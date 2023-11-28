# Weighted Residual Formulation
주어진 BVP의 WRF은 다음과 같다.  

$$ \text{find } u \in \mathcal U \quad s.t. \quad \forall w \in C^\infty_c(\Omega), \quad \int_\Omega -w\frac{d}{dx}(a\frac{du}{dx}) + wcu \thinspace dV = \int_\Omega wf \thinspace dV $$