# basic_istream

## read

파일 끝을 넘어서는 범위를 읽으라고 요청해도 오류가 발생하지 않는다. 다만, state 에 eofbit 과 failbit 이 켜지게 된다.
* https://en.cppreference.com/w/cpp/io/basic_istream/read
  * end of file condition occurs on the input sequence (in which case, setstate(failbit|eofbit) is called). The number of successfully extracted characters can be queried using gcount().

```cpp
    basic_istream& __CLR_OR_THIS_CALL read(_Elem* _Str, streamsize _Count) { 
        ios_base::iostate _State = ios_base::goodbit;
        _Chcount                 = 0;
        const sentry _Ok(*this, true);

        if (_Ok && 0 < _Count) { 
            _TRY_IO_BEGIN
            const streamsize _Num = _Myios::rdbuf()->sgetn(_Str, _Count); // _Count 만큼 읽는다.
            _Chcount              = _Num;

            // short read 면 state 만 설정한다.
            if (_Num != _Count) {
                _State |= ios_base::eofbit | ios_base::failbit; 
            }
            _CATCH_IO_END
        }

        _Myios::setstate(_State);
        return *this;
    }
```

## Eof

* https://en.cppreference.com/w/cpp/io/ios_base/iostate
  * The string input function std::getline if it completes by reaching the end of the stream, as opposed to reaching the specified terminating character.


## get

int_type get(); 형태로 함수 호출을 했을 때, 문자 읽기에 실패하면 Traits::eof() 가 반환되고 setstate(failbit | eofbit) 이 호출된다.
*  https://en.cppreference.com/w/cpp/io/basic_istream/get
*  Reads one character and returns it if available. Otherwise, returns Traits::eof() and sets failbit and eofbit.

최대 count - 1 개의 문자를 읽어온다.
* https://en.cppreference.com/w/cpp/io/basic_istream/get
* that is, reads at most std::max(0, count - 1) characters and stores them into character string pointed to by s until '\n' is found.

주어진 count 수 만큼 문자를 읽기 전에 파일이 끝나는 경우 setstate(eofbit) 이 호출된다.
* https://en.cppreference.com/w/cpp/io/basic_istream/get
* end of file condition occurs in the input sequence (setstate(eofbit) is called).

getline 함수를 맨 마지막줄에서 사용하면 setstate(eofbit) 이 호출된다.
* https://en.cppreference.com/w/cpp/string/basic_string/getline
* end-of-file condition on input, in which case, getline sets eofbit.

## tellg

<details> <summary> <h3 style="display:inline-block"> eof 상태일 때, tellg 하면 왜 failbit 이 켜지고 -1 이 return 될까?  </h3></summary>

tellg 함수가 호출이 되면 sentry 객체가 먼저 생성이 된다.
* https://en.cppreference.com/w/cpp/io/basic_istream/tellg

이 때, sentry 객체의 생성자를 보면 basic_istream 의 _Ipfx 함수를 호출해서 return 값을 _Ok 변수에 저장한다.

```cpp
    class sentry : public _Sentry_base {
    public:
        explicit __CLR_OR_THIS_CALL sentry(basic_istream& _Istr, bool _Noskip = false)
            : _Sentry_base(_Istr), _Ok(_Sentry_base::_Myistr._Ipfx(_Noskip)) {}

        explicit __CLR_OR_THIS_CALL operator bool() const {
            return _Ok;
        }

        __CLR_OR_THIS_CALL sentry(const sentry&)            = delete;
        sentry& __CLR_OR_THIS_CALL operator=(const sentry&) = delete;

    private:
        bool _Ok; // true if _Ipfx succeeded at construction
    };
```

_Ipfx 함수를 살펴보면, basic_streambuf 의 sgetc 함수나 basic_streambuf 의 snextc 함수에서 return 값이 _Traits::eof() 일 경우 setstate(ios_base::eofbit | ios_base::failbit) 함수가 호출됨을 알 수 있다.

```cpp
    bool __CLR_OR_THIS_CALL _Ipfx(bool _Noskip = false) { // test stream state and skip whitespace as needed
        if (!this->good()) {
            _Myios::setstate(ios_base::failbit);
            return false;
        }

        // state okay, flush tied stream and skip whitespace
        const auto _Tied = _Myios::tie();
        if (_Tied) {
            _Tied->flush();
        }

        bool _Eof = false;
        if (!_Noskip && this->flags() & ios_base::skipws) { // skip whitespace
            const _Ctype& _Ctype_fac = _STD use_facet<_Ctype>(this->getloc());

            _TRY_IO_BEGIN
            int_type _Meta = _Myios::rdbuf()->sgetc();

            for (;; _Meta = _Myios::rdbuf()->snextc()) {
                if (_Traits::eq_int_type(_Traits::eof(), _Meta)) { // end of file, quit
                    _Eof = true;
                    break;
                } else if (!_Ctype_fac.is(_Ctype::space, _Traits::to_char_type(_Meta))) {
                    break; // not whitespace, quit
                }
            }
            _CATCH_IO_END
        }

        if (_Eof) {
            _Myios::setstate(ios_base::eofbit | ios_base::failbit);
        }

        return this->good();
    }
```

snextc 함수에서 return 값이 _Traits::eof() 인 경우는 다음과 같다.
* meaning that input sequence has been exhausted and uflow() could not retrieve more data
* https://en.cppreference.com/w/cpp/io/basic_streambuf/snextc

sgetc 함수에서 return 값이 _Traits::eof() 인 경우는 underflow 를 호출했는데, 더이상 새로운 데이터를 채워넣을 수 없는 경우이다.
* If the input sequence read position is not available, returns underflow()
* https://en.cppreference.com/w/cpp/io/basic_streambuf/sgetc
* underflow 함수는 입력 스트림 버퍼에 새로운 데이터를 채워 넣는 함수이다. 성공하면 채워진 데이터의 첫번쨰 문자를 반환하고 실패하면 Traits::eof() 를 반환한다.
* https://en.cppreference.com/w/cpp/io/basic_streambuf/underflow
* Ensures that at least one character is available in the input area by updating the pointers to the input area (if needed) and reading more data in from the input sequence (if applicable)
* Returns the value of that character (converted to int_type with Traits::to_int_type(c)) on success or Traits::eof() on failure.

https://en.cppreference.com/w/cpp/io/ios_base/iostate 에서도 다음 상황에서 failbit 이 켜진다고 되어있다.
* The basic_istream::sentry constructor, executed at the beginning of every input function, if either eofbit or badbit is already set on the stream, or if the end of stream is encountered while consuming leading whitespace.

</details>