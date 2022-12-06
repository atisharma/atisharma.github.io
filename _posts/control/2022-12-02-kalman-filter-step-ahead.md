---
layout: post
title: Step-ahead prediction covariance for a Kalman filter
categories: systems estimation
---

Given a discrete-time autonomous system

$$\begin{align}
    x_{k+1} &= A x_k + w_k \\
    y_k &= C x_k + v_k
\end{align}$$

with

$$
    \mathcal{E}
    \left(
        \begin{array}{c}
            w \\ v
        \end{array}
    \right)
    \left(
        \begin{array}{cc}
            w' & v'
        \end{array}
    \right)
    =
    \left[
        \begin{array}{cc}
            Q & S \\
            S' & R
        \end{array}
    \right]
$$

and $$\mathcal{E} w = 0$$, $$\mathcal{E} v = 0$$.


Given a Kalman filter

$$\begin{align}
    \hat{x}_{k+1} &= A \hat{x}_k + K e_k \\
    &= (A-KC) \hat{x}_k + K y_k\\
    y_k &= C \hat{x}_k + e_k \\
    e_k :&= y_k - C \hat{x}_k,
\end{align}$$

the step-ahead state estimate error is

$$\begin{align}
    x_{k+1} - \hat{x}_{k+1} &= A x_k + w_{k+1} - A \hat{x}_k - K e_k \\
    &= A(x_k - \hat{x}_k) + w_{k+1} - Ke_k
\end{align}$$

and the output prediction error is

$$\begin{align}
    e_{k+1} :&= y_{k+1} - C\hat{x}_{k+1} \\
    &= C (x_{k+1} - \hat{x}_{k+1}) + v_{k+1} \\
    &= CA(x_k - \hat{x}_k) + CKe_k + Cw_{k+1} + v_{k+1}
\end{align}$$


Using the $$\delta$$-correlated property of the noise, the covariance of the step-ahead output prediction is

$$\begin{align}
    \mathcal{E} e_{k+1} e_{k+1}' =& CAQA'C' + CQC' \\
    & + R + CKRK'C' \\
    & + + CASK'C' + CKS'A'C'.
\end{align}$$
