> Stata连享会 [精彩推文1](https://gitee.com/arlionn/stata_training/blob/master/README.md)  || [精彩推文2](https://github.com/arlionn/stata/blob/master/README.md)


This is a repository maintained by DIME and containing example graphs on how to explore data sets and display results of Impact Evaluations using Stata. For information on how to contribute to the library and download codes and data sets, click on the link to GitHub below.

# IE Visual Library in Stata

## [Bar plots](https://github.com/worldbank/Stata-IE-Visual-Library/tree/master/Library/Bar%20plots)

### [Bar plot of two variables by treatment](https://github.com/worldbank/Stata-IE-Visual-Library/tree/master/Library/Bar%20plots/Bar%20plot%20of%20two%20variables%20by%20treatment)

![plot](http://upload-images.jianshu.io/upload_images/7692714-759b2d2fa010e02e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

```
# d;
graph bar w_total_val_harvested_a w_total_val_inputs_a, 
	  over(treated)		
	  bargap(-30)
	  legend(label(1 "Total Value Harvested")
			 label(2 "Total Value Inputs"))
	  bar (1, color("0 102 102") ) 
	  bar (2, color("153  0 76") ) 
	  ytitle ("Value in RWF")	
	  title  ("Harvested value and Input Value")
	  subtitle ("By treatment")
	  note ("Note: Variables are winsorized at 0.01");
# d cr

```

### [Combined bar plots with two axes](https://github.com/worldbank/Stata-IE-Visual-Library/tree/master/Library/Bar%20plots/Combined%20bar%20plots%20with%20two%20axes)

![plot](http://upload-images.jianshu.io/upload_images/7692714-60080ba837bff4ca.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

```
* Create individual graphs
* ------------------------
foreach foodGroup in animal fruit grain fats veg starch beverages  processed_sugar nut_pulse_seed spices other {

	if "`foodGroup'" == "animal"			local graphTitle Animal Sourced
	if "`foodGroup'" == "fruit"			local graphTitle Fruit
	if "`foodGroup'" == "grain"			local graphTitle Grain
	if "`foodGroup'" == "veg"			local graphTitle Vegetables
	if "`foodGroup'" == "starch"			local graphTitle Starchy Foods
	if "`foodGroup'" == "processed_sugar"	local graphTitle Processed/Sugar

	twoway 	bar number_group x if food_group=="`foodGroup'", ///
		yaxis(1) ytitle("Avg. Number of Foods from" "Group Consumed Last Month", axis(1)) ///
		barwidth(.9) fintensity(inten0) lcolor(black) /// 
		xlabel(0 "0" 3 "3" 6 "6" 9 "9" 12 "12") ///
		ylabel(0 "0" 1 "1" 2 "2" 3 "3", axis(1)) || ///
		line total_exp int1mo if food_group=="`foodGroup'", ///
		yaxis(2) ytitle("Total Value of Exp." "1000 Real Tz Sh.", axis(2)) ///
		ylabel(0 "0" 500 "500" 1000 "1000" 1500 "1500" 2000 "2000" 2500 "2500", axis(2)) ///
		xlabel(3 "3" 6 "6" 9 "9" 12 "12") lwidth(1.2) ///
		title("`graphTitle'") xtitle("Month of Interview") ///
		graphregion(color(white)) bgcolor(white) ///
		legend(off) ///
		name("`foodGroup'") 

}

* Combine graphs into one
* -----------------------
graph combine 	starch animal fruit grain processed_sugar veg, ///
				graphregion(color(white)) plotregion(color(white))

```

###### Contribution: Paul Christian

## [Density plots](https://github.com/worldbank/Stata-IE-Visual-Library/tree/master/Library/Density%20plots)

### [Density plot with averages](https://github.com/worldbank/Stata-IE-Visual-Library/tree/master/Library/Density%20plots/Density%20plot%20with%20averages)

![plot](http://upload-images.jianshu.io/upload_images/7692714-e105efd9ce9d4fcc.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

```
twoway 	(kdensity revenue if post == 0, color(gs10)) ///
	(kdensity revenue if post == 1, color(emerald)), ///
	xline(`pre_mean', lcolor(gs12) lpattern(dash)) ///
	xline(`post_mean', lcolor(eltgreen) lpattern(dash)) ///
	legend(order(1 "Pre-treatment" 2 "Post-treatment")) ///
	xtitle(Agriculture revenue (BRL thousands)) ///
	ytitle(Density) ///
	bgcolor (white) graphregion(color(white))

```

###### Contribution: `@luizaandrade`

### [Shaded k-density function](https://github.com/worldbank/Stata-IE-Visual-Library/tree/master/Library/Density%20plots/Shaded%20k-density%20functions)

![plot](http://upload-images.jianshu.io/upload_images/7692714-74182e962914c381.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

```
akdensity0 beta_, gen(x) at(beta_) bwidth(.0005) //akdensity0 comes from the user-written package "akdensity"

sum beta_, d
twoway 	area x beta_ if rank>15 & beta_<(`r(p10)'), color(gs14) || ///    				  
	area x beta_ if beta_>`r(p90)' & rank<980, color(gs14) || ///      						 
	area x beta_ if rank>15 & beta_<(`r(p5)'), color(gs9) || ///    						 
	area x beta_ if beta_>`r(p95)' & rank<980, color(gs9) || ///      						 
	line x beta_ if rank>15  & rank<980, lcolor(black) || ///    							  
	(pcarrowi -20 .00299 310 .00299, lcolor(cranberry) lpattern(dash) msize(zero)) || ///     
	(pcarrowi -20 `r(mean)' 310 `r(mean)', lcolor(gs7) lpattern(dash) msize(zero)) || ///     
	(pcarrowi -20 `r(p50)' 310 `r(p50)', lcolor(gs7) lpattern(dash) msize(zero)), /// 	      
	legend(off) ///
	xtitle("2SLS Coefficient from baseline model" " ") ///
	ytitle("Density" " ") ///
	xlabel(0 "0" .00299 "NQ=.00299" `r(p50)' "Median=`median'" `r(mean)' "Mean=`mean'" .015 ".02", angle(45)) ///
	ylabel(none) ///
	bgcolor(white) graphregion(color(white))

```

###### Contribution: Paul Christian

## [Line plots](https://github.com/worldbank/Stata-IE-Visual-Library/tree/master/Library/Line%20plots)

### [Fitted line with confidence intervals and text box](https://github.com/worldbank/Stata-IE-Visual-Library/tree/master/Library/Line%20plots/Fitted%20line%20with%20confidence%20interval%20and%20text%20box)

![plot](http://upload-images.jianshu.io/upload_images/7692714-f91d89a7dc1888a5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

```
twoway 	(lfitci y_hat x_var if post == 1, color("222 235 247") lwidth(.05)) ///
	(lfitci y_hat x_var if post == 0, color(gs15)) /// 
	(lfit	x_var x_var	if post == 1, color(red) lwidth(.5) lpattern(dash)) ///
	(lfit 	y_hat x_var	if post == 0, color(gs8) lwidth(.5)) /// 
	(lfit 	y_hat x_var	if post == 1, color(edkblue) lwidth(.5)), ///
	text(5 9 "Pre-treatment" "Regression coefficent: 0`beta_pre'" "P-value of coefficent = 1: `f_pre'" ///
		 12 9 "Post-treatment" "Regression coefficent: 0`beta_post'" "P-value of coefficent = 1: 0`f_post'", ///
		 orient(horizontal) size(vsmall) justification(center) fcolor(white) box margin(small)) ///
	xtitle("Independent variable value") ///
	ytitle("Predicted value of dependent variable") ///
	legend(order (6 "Pre-treatment" 7 "Post-treatment" 3 "Pre-treatment 95%CI" 1 "Pre-treatment 95%CI")) ///
	graphregion(color(white)) bgcolor(white)

```

###### Contribution: `@luizaandrade`

## [Maps](https://github.com/worldbank/Stata-IE-Visual-Library/tree/master/Library/Maps)

### [Map displaying levels of a variable](https://github.com/worldbank/Stata-IE-Visual-Library/tree/master/Library/Maps/Map%20displaying%20levels%20of%20a%20variable)

![map](http://upload-images.jianshu.io/upload_images/7692714-1a3c2966e9aa9e6c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

```
spmap jobs_scarce_code using world_shape_coord if admin!="Antarctica", id(id) ///
fcolor(Reds) osize(.1) ocolor(black) ///
clmethod(custom)  clbreaks(0 .2 .40 .6 .8 1)  ///
legend(position(8) ///
region(lcolor(black)) ///
label(1 "No data") ///
label(2 "0% to 20%") ///
label(3 "20% to 40%") ///
label(4 "40% to 60%") ///
label(5 "60% to 80%") /// 
label(6 "80% to 100%")) ///
legend(region(color(white))) ///
plotregion(icolor(bluishgray)) ///
title("When jobs are scarce, men should have more of a right to a job than women.") ///
subtitle("Agreement with the statement above by country") ///
note("Source: World Values Survey (2014 or last available year)") ///
saving(map, replace)
graph export map.png, as(png)  replace

```

###### Contribution: `@marcelamello90`

## [RD plots](https://github.com/worldbank/Stata-IE-Visual-Library/tree/master/Library/RD%20plots)

### [Regression discontinuity plot with confidence intervals](https://github.com/worldbank/Stata-IE-Visual-Library/tree/master/Library/RD%20plots/Regression%20discontinuity%20plots%20with%20confidence%20intervals)

![rdplot1](http://upload-images.jianshu.io/upload_images/7692714-0d556f6341467240.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

```
twoway  (lpolyci tmt_status pmt_score if pmt_score < `cutoff', clcolor(navy) acolor(gs14)) ///
	(lpolyci tmt_status pmt_score if pmt_score > `cutoff', clcolor(navy) acolor(gs14)), ///
	xline(`cutoff', lcolor(red) lwidth(vthin) lpattern(dash)) ///
	ytitle(Probability of receiving treatment) ///
	xtitle(Proxy means test score) ///
	legend(off) ///
	bgcolor (white) graphregion(color(white))  ///
	note("Note: gray area is 95% confidence interval.")

```

###### Contribution: `@luizaandrade`

## [Scatter plots](https://github.com/worldbank/Stata-IE-Visual-Library/tree/master/Library/Scatter%20plots)

### [Scatter plot with fitted line](https://github.com/worldbank/Stata-IE-Visual-Library/tree/master/Library/Scatter%20plots/Scatter%20plot%20with%20fitted%20line)

![scatterplot1](http://upload-images.jianshu.io/upload_images/7692714-8435f8954afc55f5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

```
twoway 	(scatter revenue area_cult if post == 0, msize(vsmall) mcolor(gs14)) ///
	(lfit revenue area_cult if post == 0, color(gs12)) ///
	(scatter revenue area_cult if post == 1, msize(vsmall) mcolor(stone)) ///
	(lfit revenue area_cult if post == 1, color(sand)), ///
	ytitle(Agriculture revenue (BRL thousands)) ///
	xtitle(Cultivated area) ///
	legend(order(2 "Pre-treatment" 4 "Post-treatment")) ///
	bgcolor (white) graphregion(color(white))

```

###### Contribution: `@luizaandrade`

### [Scatter plot with fitted line and confidence interval](https://github.com/worldbank/Stata-IE-Visual-Library/tree/master/Library/Scatter%20plots/Scatter%20plot%20with%20fitted%20line%20and%20confidence%20interval)

![scatterplot2](http://upload-images.jianshu.io/upload_images/7692714-17d0cd8f3a8ab6ac.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

```
twoway ///
(lfitci jobs_scarce_code avg_growth ) ///
(scatter jobs_scarce_code avg_growth if continent == "Africa", mcolor(cranberry) m(O) )  ///
(scatter jobs_scarce_code avg_growth if continent == "Asia",   mcolor(dkgreen) m(D) ) ///
(scatter jobs_scarce_code avg_growth if continent == "Europe", mcolor(ebblue ) m(T) ) ///
(scatter jobs_scarce_code avg_growth if continent == "North America", mcolor(dkorange) m(O)) ///
(scatter jobs_scarce_code avg_growth if continent == "Oceania", mcolor(navy) m(D) ) ///
(scatter jobs_scarce_code avg_growth if continent == "South America", mcolor(red) m(T)),  ///
xlabel(-5(5)15) 		///
xtitle("Average Annual GDP per Capita Growth Rate (%)", axis(1)) ///
ylabel(0(0.2)1) ///
ytitle("Gender Value Indicator" ) ///
legend(order( 3 4 5 6 7 8) label(3 "Africa") label(4 "Asia")  label(5 "Europe") ///
label(6 "North America") label(7 "Oceania") label(8 "South America")    ///
ring(0) position(4)) ///
title("Gender Value Indicator and GDP per Capita Growth" "Correlation")  ///
note("Source: World Values Survey (2014 or last available year) and World Bank") /// 
graphregion(color(white)) bgcolor(white)

```

###### Contribution: `@marcelamello90`

### [Scatter plot with polynomial smoothing and confidence interval](https://github.com/worldbank/Stata-IE-Visual-Library/tree/master/Library/Scatter%20plots/Scatter%20plot%20with%20polynomial%20smoothing%20and%20confidence%20intervals)

![plot](http://upload-images.jianshu.io/upload_images/7692714-52753db31017f000.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

```
***Create First Graph
sum cons_pae_m_sine, det
twoway scatter cons_pae_sd_sine cons_pae_m_sine if cons_pae_m_sine < `r(p99)' ///
	|| lpolyci cons_pae_sd_sine cons_pae_m_sine if cons_pae_m_sine < `r(p99)', ///
	legend(off) /// 
	xtitle(" " "`=ustrunescape("\u006D\u0302")'", size(large)) /// 		m-hat
	ytitle("`=ustrunescape("\u0073\u0302")'" " ", size(large)) /// 		s-hat 
	xlabel(50 "50" 100 "100" 150 "150" 200 "200") ///	
	graphregion(color(white)) bgcolor(white) ///
	name(s_by_mhat)

***Create Second Graph
sum cons_pae_m_sine, det
twoway scatter cv cons_pae_m_sine if cons_pae_m_sine<`r(p99)' & cons_pae_m_sine>`r(p1)' ///
	|| lpolyci cv cons_pae_m_sine if cons_pae_m_sine<`r(p99)' & cons_pae_m_sine>`r(p1)', ///
	mcolor(maroon) ///
	ytitle("`=ustrunescape("\u0073\u0302/\u006D\u0302")'" " ", size(large)) /// 	s-hat/m-hat
	xtitle(" " "`=ustrunescape("\u006D\u0302")'", size(large)) ///					m-hat
	legend(order(2 3) label(3 "Local Poly.") label(2 "95% CI")) ///
	graphregion(color(white)) bgcolor(white) ///
	name(cv_by_mhat)

***Combine graphs
grc1leg s_by_mhat cv_by_mhat, ///
	row(1) legendfrom(cv_by_mhat) ///
	imargin(0 0 0 0) graphregion(margin(t=0 b=0)) ///
	position(6) fysize(75) fxsize(150) ///
	graphregion(color(white)) plotregion(color(white))

```

###### Contribution: Paul Christian


> Stata连享会 [精彩推文1](https://gitee.com/arlionn/stata_training/blob/master/README.md)  || [精彩推文2](https://github.com/arlionn/stata/blob/master/README.md)


