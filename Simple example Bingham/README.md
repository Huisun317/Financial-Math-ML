# RealNVP-application-Normalizing-flow
We give some examples of applying realNVP to problems of interest. 
1. We provide a simple example of sampling from the Bingham distribution using realNVP. 

### Some brief introdunction and problem formulation

One of the major goal in stochastic computing is sampling, i.e. given a density $\nu(dx)$, how to efficiently sample random variables from such a distribution. Suppose that 
	$$\nu(dx) = \rho(x)dx$$ 
for some density $\rho(x)$. 

In this work, we propose a transportation method to do such sampling. That is, with the information that it's difficult to sample from the distribution $\nu(dx)$, we want to find a transport map 
$$T_*:\Omega \rightarrow \Omega$$ 

such that the following equation holds:
            $$ \int_{\Omega} O(T_* (x)) \rho_B(x)dx = \int_{\Omega} O(x) \rho_* (x)dx $$  	    
We comment that the existence of the map $T_* $ is guaranteed by the optimal transportation theory.  To distinguish between the base space and the target domain, we use $\Omega_x $ and $\Omega_y$ to denote $\Omega$ even though these two are the same. 

We denote by $\bar{T}$ the inverse of the exact map $T_* $, then by a change of coordinate in 

$$\begin{align}
\int_{\Omega_x} O(T_* (\bar{T}(y)))\rho_B(\bar{T}(y)) d\bar{T}(y) & = \int_{\Omega_y} O(y) \rho_B(\bar{T}(y)) \det|\nabla_y \bar{T}(y)| dy \\
	                                                          & = \int_{\Omega_y} O(y) \rho_* (y) dy
\end{align}$$

As a result, we have the following relationship
	$$\rho_B(\bar{T}(y)) \det|\nabla_y \bar{T}(y)| =\rho_* (y)$$
Now the question is how to construct such map $T$ that can approximate the true map $T_* $. W Now our goal is to construct an approximation of the transport map $T_* $. 

We let $z:=T(z_0)=f_{k} \circ f_{k-1} \circ ... \circ f_{2} \circ f_{1}(z_0)$, that is the composition of k elementary maps which is the appromxiation of the optimal map $T_* $. We name the density on the lefthand side of the equation $\hat{\rho}(y)$ (with $\bar{T}$ replaced by $T^{-1}$).


To measure the quality of the approximation $\hat{\rho}$ to $\rho_* $, we use the KL-divergence. 

$$\begin{align}
	D_{KL}(\hat{\rho}||\rho_* )&= \int \hat{\rho} \log \frac{ \hat{\rho}}{\rho_* } dx \\
	&=\int \hat{\rho} \log \hat{\rho} dy-\int \hat{\rho} \log \rho_* dy
\end{align}$$

Now for the first part, we have 

$$\begin{align}
	\int \hat{\rho} \log \hat{\rho} dy = \mathbb{E}_ {\hat{\rho}}  [\log \hat{\rho}(z) ]
\end{align}$$

where $z \sim \hat{\rho}(x) dx$. 
To be more specific

$$\begin{align}
	\hat{\rho}(z)&=\rho_B(T^{-1}(z)) \det|\nabla_z T^{-1}(z)|  \\
	&=\rho_B(z_0)/ \det|\nabla_z T(z)|
\end{align}$$

As a result, we have 

$$\begin{align}
	\mathbb{E}_ {\hat{\rho}}[\log \hat{\rho}(z)]&=\mathbb{E}_ {\rho_0}[\log(\rho_B(z_0))- \sum^k_{i=1} \log\det |\nabla_{z_{i-1}}f_i(z_{i-1}) |]
\end{align}$$

The second term in \eqref{kl_difference}

$$\begin{align}
	-\mathbb{E}_ {\hat{\rho}}[\log \rho_* (z)] &= - \mathbb{E} {\rho_0}[\log \rho_* (T(z_0))]
\end{align}$$

where $z_0 \sim \rho_0$.

