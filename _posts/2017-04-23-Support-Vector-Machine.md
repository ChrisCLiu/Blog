Support vector machine is a kind of advanced classification method. Basic SVM is used as a binary classification model. Its main propose is finding a linear expression of features and saperating datas into two classification as far as possible. That is, SVM tries to find a hyperplane that maximize the margin between the boundary of two data groups. The vector from the hyperplane to the data at the boundary is so-called support vector.

### Relationship to Logistic Regression
SVM is a linear classification model. We need to find a hyperplane $H$ such that 

$$H: \omega^T x + b = 0$$

We define $\hat y$ as the classification if $\omega^T x + b<0$, $y$ is classified to -1 and if $\omega^T x + b>0$, $y$ is classified to 1. This kind of classification is inherited from 0-1 classification from [Logistic Regression](http://blog.xliu.info/Logistic-Regression). -1-1 is used here for the convenience of calculaton. 

We will compare the loss functions of LR and SVM in [Hinge Loss](#hinge-loss)

### A Simple Example for Linear Classification. 
Again, we recall that the propose of SVM is to find a hyperplane $H$ so that could maxified the margin, which is so-called *Marximum Margin Classifier*. In the picture below, we showed in detail what is SVM 

<div align="center">
<img src="https://ooo.0o0.ooo/2017/04/23/58fc6c39569bf.png" alt="SVM.png" title="SVM.png"  />
</div>

As we can see in the picture, two group of items are located at different sides of the line $\omega^Tx+b = 0$, which is our hyperplane $H$ in this example. The dotted lines are paralleled to the hyperplane $H$, and they are the *classification boundary*. We call the data record located on the boundary as ***support vector*** and therefore the support vector satisfied the condition $y(\omega^Tx+b) = 1$. Our job in SVM is maxified the *Gap*, which is $\dfrac{2}{\omega^T\omega}$. As we can see, the larger the gap is, the further two classifications will be.

### Cost Function & Coefficient Estimation
Although the example above is a very simple explanation to SVM, but the propose is generalized. We have to maxified the gap to get the classification. 

Therefore, our goal is 

$$\max\ \dfrac{1}{\mid\mid\omega\mid\mid}\quad s.t.\ y^{(i)}(\omega^Tx^{(i)}+b)\ge 1,\ i=1,2,...m$$

In order to get the maximum of $\dfrac{1}{\mid\mid\omega\mid\mid}$, we calculate the minimum of $\dfrac{1}{2}\mid\mid\omega\mid\mid^2$. Then our goal is 

$$\min\ \dfrac{1}{2}\mid\mid\omega\mid\mid^2\quad s.t.\ y^{(i)}(\omega^Tx^{(i)}+b)\ge 1,\ i=1,2,...m$$

(SVM中为了方便计算进行了一系列变换，从之前认定分类边界的值为1，到现在的最大化问题转变为最小化问题，都是为了之后进行计算时方便。至于为什么转成了二次最小化问题，还是为了求导方便。)

Then the problem has tranfered to a optimizing problem under certain constraints. To handle the constraints, we introduce *Lagrange Multiplier* to transfer the problem to its ***dual problem***. The ***Lagrange function*** is defined as,

$$\mathcal{L}(\omega,b,\alpha) = \dfrac{1}{2}\mid\mid\omega\mid\mid^2 - \sum\limits_{i=1}^{n}\alpha_i(y^{(i)}(\omega^Tx^{(i)}+b)-1)$$

Then our cost function is

$$J(\omega) = \max \limits_{\alpha_i\ge 0}\  \mathcal{L}(\omega,b,\alpha)$$

Obviously, if one constraint is not statisfied, then $J(\omega)$ will be infinite. Now, we transfer our problem into its dual problem.

Original Problem,

$$\min\limits_{\omega,b}J(\omega) = \min\limits_{\omega,b}\max\limits_{\alpha_i\ge 0}\  \mathcal{L}(\omega,b,\alpha) = p^*$$

Dual Problem,

$$\max\limits_{\alpha_i\ge 0} \min\limits_{\omega,b}\ \mathcal{L}(\omega,b,\alpha) = d^* $$

$ p^{ * } $ and $d^{ * } $ is the optimization solution to original problem and dual problem, and we have relation $p^{ * } \ge \mathcal{L}(\omega,b,\alpha) \ge d^{ * }$.

We call $\Delta = p^{ * } - d^{ * } $ as ***duality gap***, when $\Delta$ is 0, we call the problems have ***strong duality***. If and only if at least one possible solution exsits, that is, we have strong duality, then the KKT condition must be satisfied.

##### KTT Condition
> 
> If we have a constaint optimization problem defined as below,
> 
> $$\min f(x)\quad s.t.\  h_j(x) = 0,\ j=1,...,p\ \text{and}\ g_k(x) \le 0,\ k=1,...,q$$
> 
> We define Lagrange function as, 
> 
> $$\mathcal{L}(x,\lambda,\mu) = f(x) + \sum\limits_{j=1}^{p}\lambda_jh_j(x) + \sum\limits_{k=1}^{q}\mu_kg_k(x)$$
> 
> The optimized solution $x^* $ should satisfy the following conditions,
> 
>  $$\dfrac{\partial \mathcal{L}}{\partial x}\Bigg|_{x=x^*} = 0,$$
> 
> $$\lambda_j \neq 0, \quad \mu_k \ge 0,\ \text{and} \quad \mu_kg_k(x^*) = 0$$
> 
> 
> $$h_j(x^* )=0,\ j=1,...,p \quad \text{and}\quad g_k(x^*) \le 0,\ k=1,...,q $$

Come back to our lagrange function, we do a partial derivative to both $\omega$ and $b$, then we get 

$$\dfrac{\partial \mathcal{L}}{\partial \omega} = 0 \rightarrow \omega = \sum\limits_{i=1}^m \alpha_iy^{(i)}x^{(i)} $$

$$\dfrac{\partial \mathcal{L}}{\partial b} = 0 \rightarrow \sum\limits_{i=1}^m \alpha_iy^{(i)} = 0$$

Apply them to larange function, we get 

$$ \mathcal{L}(\omega,b,\alpha) = \sum\limits_{i=1}^m \alpha_i - \dfrac{1}{2}\sum\limits_{i,j=1}^m\alpha_i\alpha_jy^{(i)}y^{(j)}(x^{(i)})^Tx^{(j)} $$

Therefore, we have the final optimization problem only having $\alpha$ as parameter.

$$\max\limits_{\alpha}\ \sum\limits_{i=1}^m \alpha_i - \dfrac{1}{2}\sum\limits_{i,j=1}^m\alpha_i\alpha_jy^{(i)}y^{(j)}(x^{(i)})^Tx^{(j)}$$

$$ s.t.\ \quad \alpha_i \ge 0,\ i = 1,...,m \  \quad \text{and} \quad \sum\limits_{i=1}^{m} \alpha_i y^{(i)} = 0 $$

Once we get $\alpha$, we can easily calculate $\omega$ and $b$ by following expression

$$ \omega^* = \sum\limits_{i=1}^m \alpha_iy^{(i)}x^{(i)} $$

$$ b^* = \dfrac{ \max_{i:\ y^{(i)}=-1} \omega^{ * T} x^{(i)} + \min_{i:y^{(i)}=1}\omega^{ * T}x^{(i)}}{2} $$

### Kernel Function & Non-Linearity
Obviously, our SVM now can only handle the linear classification problem. When the data is not linear classified, we introduce kernel function $\kappa(·,·)$. kernel function map the low-dimentional data into a higher dimention, therefore, if map properly, the data will be linear claasifiable.

Take a simple example, say, we have a function $\phi(x)$ in $\mathbb{R}^5$. Data on $\mathbb{R}^2$ are distributed as two concentric circles, the features are $X_1$ and $X_2$. Then  $\varphi(X_1, X_2) = \sum\limits_{i=1}^5\alpha_iZ_i + \alpha_0 = 0$ where $Z_1 = X_1,\ Z_2 = X_2,\ Z_3 = X_1^2,\ Z_4 = X_2^2,$ and $Z_5 = X_1X_2$. Hence, the data becomes linear classifiable. Therefore, we only need to calculate $(\phi(x)^{(i)})^T\phi(x)^{(j)}$ instead of $(x^{(i)})^Tx^{(j)}$.

However, there is an obvious problem -- the calculation. The map function will no doubt lead to a dimensional disaster. Therefore, we need kernel function.

Take a simple example. we assume $x_1 = (\eta_1, \eta_2)$ and $x_2 = {\xi_1,\xi_2}$. $\phi(·)$ is a map from $\mathbb{R}^2$ to $\mathbb{R}^5$ as introduced above.

$<\phi(x_1),\phi(x_2)> = \eta_1\xi_1 + \eta_1^2\xi_1^2 + \eta_2\xi_2 + \eta_2^2\xi_2^2 + \eta_1\eta_2\xi_1\xi_2$

we note that $\kappa(x_1,x_2) = (< x_1,x_2 >+1)^2 = 2\eta_1\xi_1 + \eta_1^2\xi_1^2 + 2\eta_2\xi_2 + \eta_2^2\xi_2^2 + 2\eta_1\eta_2\xi_1\xi_2 + 1$

Campare to the map $\varphi$, all we need to do is adjust some parameters and add a extra dimension 1.

This means that we could do the calculation in the low dimension and get the same result of the calculation in hign dimension with a proper kernel function! The kernel avoids the computation in high dimension and simplifies the calculation of inner product in it. There is another "coincidence" that all our calculation of $x$ is SVM is inner product!

***Gaussian kernel*** $\kappa(x_1,x_2) = exp(-\dfrac{\mid\mid x_1-x_2\mid\mid^2}{2\sigma^2})$ is a frequently used kernel function, it map to a infinite dimension space. A large $\sigma$ will punish the high dimension features hard and make it actually a low dimensional subspace. A small $\sigma$ will map every dataset to a linear classifiable space, certainly, it will bring a severe overfitting problem.

***Polynomial kernel*** $\kappa(x_1,x_2) = (< x_1,x_2 >+R)^d$ is another known kernel functionof degree $d$, it map to a $\binom{m+d}{m}$ dimensional space. 

When a support vector classifier is combined with a non-linear kernel like the above two, the resulting classifier is known as a ***support vector machine***.


### Potential Extensions

#### Slack Variables to Handle Outliers
If the data records are not strictly seperated, say, some naughty data point located too far away from its group and too close to its opponents' group, the origin SVM will suffer a bad result. Therefore, slack variables are introduced.

We still do a SVM but only ignore the naughty ones, that is we introduce $\xi_i$ as slack variables into the constraints. that is 

$$ y^{(i)} (\omega^Tx^{(i)}+b) \ge 1-\xi_i$$ 

$\xi_i$ allows data point $x^{(i)}$ to be away from the boundary, but we still need them to be as small as possible, therefore, in origin goal, we need add one more term,

$$\min\ \dfrac{1}{2}\mid\mid\omega\mid\mid^2 + C\sum\limits_{i=1}^m \xi_i\quad$$

$$s.t.\ y^{(i)}(\omega^Tx^{(i)}+b)\ge 1-\xi_i,\ \text{and}\ \xi_i\ge 0 \quad i=1,2,...m$$ 


where $C$ is used to balance the weight between find the hyperplane and allow the error.

The lagrance function changes to 

$$\mathcal{L}(\omega,b,\xi,\alpha,r) = \dfrac{1}{2}\mid\mid\omega\mid\mid^2 + C\sum\limits_{i=1}^{m}\xi_i - \sum\limits_{i=1}^{n}\alpha_i(y^{(i)}(\omega^Tx^{(i)}+b)-1+\xi_i) - \sum\limits_{i=1}^{m}r_i\xi_i$$

Use same analyze strategy as before, we get

$$\dfrac{\partial \mathcal{L}}{\partial \omega} = 0 \rightarrow \omega = \sum\limits_{i=1}^m \alpha_iy^{(i)}x^{(i)} $$

$$\dfrac{\partial \mathcal{L}}{\partial b} = 0 \rightarrow \sum\limits_{i=1}^m \alpha_iy^{(i)} = 0$$

$$\dfrac{\partial \mathcal{L}}{\partial \xi} = 0 \rightarrow C-\alpha_i-r_i = 0,\ i = 1,...,m$$

Because $C-\alpha_i-r_i = 0$ and $r_i\ge 0$ (as the request of being Lagrange multiplier) Then we get the dual problem

$$\max\limits_{\alpha}\ \sum\limits_{i=1}^m \alpha_i - \dfrac{1}{2}\sum\limits_{i,j=1}^m\alpha_i\alpha_jy^{(i)}y^{(j)}(x^{(i)})^Tx^{(j)}$$

$$ s.t.\ \quad 0 \le \alpha_i \le C,\ i = 1,...,m \  \quad \text{and} \quad \sum\limits_{i=1}^{m} \alpha_i y^{(i)} = 0 $$

#### Multi-Classification
Though many proposals for extending SVM to the $K$-class case have been made, the two most popular are the ***one-versus-one*** and ***one-verssus-all***. Brief introduction will shows as below

##### One-Versus-One Classification
When there are $k$ classes, this method constructs $\binom{k}{2}$ SVMs, each of which compares a pair of classes, which is every time there are only two classes are choosed to compare. We classify a test observation using each of these SVMs and the final classification is performed by assigning the test observation to the class to which it was most frequently assigned in these pairwise classifications.

##### One-Versus-All Classification
When there are $k$ classes, this method constructs $k$ SVMs, each of which compares one against others, which is when one class is classified, anther $k-1$ classes are considered as a whole. We classify a test observation using each of these SVMs and the final classification is performed by finding out the cost-least-classification.

### Hinge Loss
The cost function in SVM is called ***Hinge Loss***, represented as 

$$\max(0, 1-t_iy^{(i)})$$

where $t_i$ is the right label of the data point $x^{(i)}$. This loss function is closely related to ***logarithm loss*** used in logistic regression, which is represented as

$$-log(P(Y\mid X))$$

*Hinge loss* and *logarithm loss* are similar but when the data is classified as a right point, the former ignores its cost but the latter still counts as a very small value, the curve showed the cost when the right class is $y=1$

The comparasion showed as below,
<div align="center">
<img src="https://ooo.0o0.ooo/2017/04/23/58fcb4ef5474e.png" alt="SVM_Loss.png" title="SVM_Loss.png" />
</div>

### SMO Algorithm

To be continue...