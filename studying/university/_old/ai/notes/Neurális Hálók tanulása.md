---
aliases: 
tags:
  - ai
cssclasses:
  - center-images
  - center-titles
  - markdown-preview-view
created: 2024-12-10 21:11
---
#  [[Neurális Hálók]] tanítása


>[!summary]+ 
>- Maximum likelihood becslés:
>  $$
> \theta_{ML}=\arg \max_{\theta}P(X|\theta)=\arg \max_{\theta} \prod_{i}P_{\theta}(X_{i})
>$$
> - Maximum *feltételes* likelihood becslés:
>$$
>\theta^{*}=\arg \max_{\theta}P(Y|X,\theta)=\arg \max_{\theta} \prod_{i}P_{\theta }(y_{i}|x_{i})
>$$
>$$
>l(w)=\prod_{i} \frac{e^{w_{y_{i}}\cdot x_{i}}}{\sum_{y}e^{w_{y}\cdot x_{i}}}
>$$
>$$
>ll(w) = \sum_{i}\log P_{w}(y_{i}|x_{i})=\sum_{i}w_{y_{i}}\cdot x_{i}-\log \sum_{y}e^{w_{y}\cdot x_{i}}
>$$
>
>Többosztályos logisztikus regresszió:
>$$
>\max_{w}ll(w)=max_{w}\sum_{i}\log P(y^{(i)}|x^{(i)};w)
>$$
>$$
>P(y^{(i)}|x^{(i)};w)=\frac{e^{w_{y^{(i)}}\cdot f(x^{(i)})}}{\sum_{y}e^{w_{y}}\cdot f(x^{(i)})}
>$$











