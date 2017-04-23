Support vector machine is a kind of advanced classification method. Basic SVM is used as a binary classification model. Its main propose is finding a linear expression of features and saperating datas into two classification as far as possible. That is, SVM tries to find a hyperplane that maximize the margin between the boundary of two data groups. The vector from the hyperplane to the data at the boundary is so-called support vector.

### Logistic Regression
SVM is a linear classification model. We need to find a hyperplane $H$ such that 

$$H: \omega^T x + b = 0$$

We define $\hat y$ as the classification if $\omega^T x + b<0$, $y$ is classified to -1 and if $\omega^T x + b>0$, $y$ is classified to 1. [Logistic Regression](http://blog.xliu.info/Logistic-Regression) use 0 and 1 for different labels. In SVM, we use -1 and 1 just for the convenience of calculation.

### A Simple Example for Linear Classification. 
