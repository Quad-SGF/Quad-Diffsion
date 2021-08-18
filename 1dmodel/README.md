## One-Dimensional Model

We design a 1-dimensional model that has loss function
    

![math](https://render.githubusercontent.com/render/math?math=L%28w%29%20%3D%20%5Cbegin%7Bcases%7D%0A%20%20%20%20-%5Cell%28w-a%29%20%2B%5Cell%28-a%29%20%26%20%5Ctext%7B%2C%7D%20%20w%20%5Cgeq%200%20%5C%5C%20%0A%20%20%20%20%5Cell%28-w-a%29%20-%5Cell%28-a%29%26%20%5Ctext%7B%2C%7D%20w%20%3C%200%20%5Cend%7Bcases%7D%2C)


where  ![math](https://render.githubusercontent.com/render/math?math=%5Cell%28w%29%20%3D%20%5Csum_%7Bi%3D1%7D%5EN%20%28wx_i-y_i%29%5E2) ,  ![math](https://render.githubusercontent.com/render/math?math=x_i%2Cy_i%5Csim%20%5Cmathcal%7BN%7D%280%2C1%29) ,  ![math](https://render.githubusercontent.com/render/math?math=N%3D500)  and  ![math](https://render.githubusercontent.com/render/math?math=a)  is a hyperparameter. We can adjust barrier height  ![math](https://render.githubusercontent.com/render/math?math=%5CDelta%20L)  by parameter  ![math](https://render.githubusercontent.com/render/math?math=a)  without changing the Hessian at minima and saddle point. 

The curve of  ![math](https://render.githubusercontent.com/render/math?math=L%28w%29)  with  ![math](https://render.githubusercontent.com/render/math?math=%5CDelta%20L%20%3D%201)  is plotted below.

<img src="./figures/loss-curve.png" width="600">

We set barrier height  ![math](https://render.githubusercontent.com/render/math?math=%5CDelta%20L)  as  ![math](https://render.githubusercontent.com/render/math?math=%5C%7B0.5%2C0.6%2C%5Ccdots%2C%202%5C%7D) . For each height, we train the model by  ![math](https://render.githubusercontent.com/render/math?math=%5Ctext%7BSGD%7D)  and the Euler-Maruyama discretization of  ![math](https://render.githubusercontent.com/render/math?math=%5Ctext%7BSGF%7D_%7B%5CSigma_2%5E%7Bw%5E%2A%7D%7D)  and  ![math](https://render.githubusercontent.com/render/math?math=%5Ctext%7BSGF%7D_%7B%5CSigma%5E%7Bw%5E%2A%7D_0%7D) , and record the total number of the iteration when the model firstly escaped from the local minimum bypassing the saddle point. We repeat the training for 300 rounds for every selected barrier height and report the averaged escaping time in following figure.
<img src="./figures/MEFT.png" width="600">

<!-- <p float="left">
<img src="./figures/SGD.png" width="250"/>
<img src="./figures/SGF2.png" width="250"/>
<img src="./figures/SGF0.png" width="250"/>
</p> -->
As shown above,  ![math](https://render.githubusercontent.com/render/math?math=%5Ctext%7BSGF%7D_%7B%5CSigma_2%5E%7Bw%5E%2A%7D%7D)  can mimic the escaping behavior of  ![math](https://render.githubusercontent.com/render/math?math=%5Ctext%7BSGD%7D) , and the averaged escaping time of  ![math](https://render.githubusercontent.com/render/math?math=%5Ctext%7BSGD%7D)  grows in polynomial order of the barrier height  ![math](https://render.githubusercontent.com/render/math?math=%5CDelta%20L)  which matches the theoretical result for  ![math](https://render.githubusercontent.com/render/math?math=%5Ctext%7BSGF%7D_%7B%5CSigma_2%5E%7Bw%5E%2A%7D%7D)  while that of  ![math](https://render.githubusercontent.com/render/math?math=%5Ctext%7BSGF%7D_%7B%5CSigma_0%5E%7Bw%5E%2A%7D%7D)  grows in exponential order.

To explicitly show this, we conduct regressions between mean escaping time, which is denoted as  ![math](https://render.githubusercontent.com/render/math?math=%5Ctau) , and barrier height  ![math](https://render.githubusercontent.com/render/math?math=%5CDelta%20L) . The results are shown below, from which we can observe that the R-square of these regressions are very high ( ![math](https://render.githubusercontent.com/render/math?math=%5Cgeq%200.99) ) and thus all of them are well fitted. From the results,  ![math](https://render.githubusercontent.com/render/math?math=%5Clog%28%5Ctau%29%20%3D%20%5Cmathcal%7BO%7D%28%5Clog%28%5CDelta%20L%29%29)  holds for both  ![math](https://render.githubusercontent.com/render/math?math=SGD)  and  ![math](https://render.githubusercontent.com/render/math?math=%5Ctext%7BSGF%7D_%7B%5CSigma_2%5E%7Bw%5E%2A%7D%7D) , which means  ![math](https://render.githubusercontent.com/render/math?math=%5Ctau%20%5Cpropto%20%7B%5CDelta%20L%7D%5Ea)  and therefore the mean escaping time of  ![math](https://render.githubusercontent.com/render/math?math=%5Ctext%7BSGD%7D)  and  ![math](https://render.githubusercontent.com/render/math?math=%5Ctext%7BSGF%7D_%7B%5CSigma_2%5E%7Bw%5E%2A%7D%7D)  grows in polynomial order; For  ![math](https://render.githubusercontent.com/render/math?math=%5Ctext%7BSGF%7D_%7B%5CSigma%5E%7Bw%5E%2A%7D_0%7D) , we have  ![math](https://render.githubusercontent.com/render/math?math=%5Clog%28%5Ctau%29%20%3D%20%5Cmathcal%7BO%7D%28%5CDelta%20L%29)  that means  ![math](https://render.githubusercontent.com/render/math?math=%5Ctau%20%5Cpropto%20e%5E%7B%5CDelta%20L%7D) , which is exponential order.


   ![math](https://render.githubusercontent.com/render/math?math=%5Ctext%7BSGD%7D)  :  ![math](https://render.githubusercontent.com/render/math?math=%5Clog%28%5Ctau%29%20%3D%20%5Cmathcal%7BO%7D%28%5Clog%28%5CDelta%20L%29%29)        |   ![math](https://render.githubusercontent.com/render/math?math=%5Ctext%7BSGF%7D_%7B%5CSigma_2%5E%7Bw%5E%2A%7D%7D)  :  ![math](https://render.githubusercontent.com/render/math?math=%5Clog%28%5Ctau%29%20%3D%20%5Cmathcal%7BO%7D%28%5Clog%28%5CDelta%20L%29%29)           |  ![math](https://render.githubusercontent.com/render/math?math=%5Ctext%7BSGF%7D_%7B%5CSigma%5E%7Bw%5E%2A%7D_0%7D)  :  ![math](https://render.githubusercontent.com/render/math?math=%5Clog%28%5Ctau%29%20%3D%20%5Cmathcal%7BO%7D%28%5CDelta%20L%29) 
-------------------------|-------------------------|-------------------------
![](./figures/SGD.png)  |  ![](./figures/SGF2.png) | ![](./figures/SGF0.png) 

