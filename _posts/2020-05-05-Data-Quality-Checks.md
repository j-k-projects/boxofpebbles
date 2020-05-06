---
title: "Quality Checks May 5th 2020"
toc: true
layout: post
description: Quality check models.
categories: [stata]
---

# Data and Model Quality

Loading the data. This new dataset includes the new scaled measures to use as dependent variables.

```stata
use "New_Data_May_5th_2020"
```

Running a regression

```stata

foreach var in natural{
  qui reg `var' i.condition
  eststo naturaleee
  esttab naturaleee, html label title("`: var label `var''") ///
  nobaselevels ///
  cells((b(f(2) star) p(f(3))) se(f(2) par)) ///
  starlevels(* .10 ** .05 *** .01) ///
  collabels("Coefficient" "p-value") ///
  coeflabels(_cons "Constant (Control)") ///
  aic bic legend
}
```

Output




<table border="0" width="*">
<caption>Animal or Lab Origin</caption>
<tr><td colspan=3><hr></td></tr>
<tr><td>                    </td><td>         (1)   </td><td>            </td></tr>
<tr><td>                    </td><td>Animal or Lab Origin   </td><td>            </td></tr>
<tr><td>                    </td><td> Coefficient   </td><td>     p-value</td></tr>
<tr><td colspan=3><hr></td></tr>
<tr><td>Natural Origin      </td><td>       -0.38** </td><td>       0.014</td></tr>
<tr><td>                    </td><td>      (0.15)   </td><td>            </td></tr>
<tr><td>Chinese Conspiracy  </td><td>        0.34** </td><td>       0.024</td></tr>
<tr><td>                    </td><td>      (0.15)   </td><td>            </td></tr>
<tr><td>Competitive Frame   </td><td>        0.33** </td><td>       0.032</td></tr>
<tr><td>                    </td><td>      (0.15)   </td><td>            </td></tr>
<tr><td>Constant (Control)  </td><td>        3.34***</td><td>       0.000</td></tr>
<tr><td>                    </td><td>      (0.11)   </td><td>            </td></tr>
<tr><td colspan=3><hr></td></tr>
<tr><td>Observations        </td><td>        1103   </td><td>            </td></tr>
<tr><td><i>AIC</i>          </td><td>      4414.1   </td><td>            </td></tr>
<tr><td><i>BIC</i>          </td><td>      4434.1   </td><td>            </td></tr>
<tr><td colspan=3><hr></td></tr>
<tr><td colspan=3>
* p<.10, ** p<.05, *** p<.01
</td></tr>
</table>




### Running Model Checks

```stata
%html
foreach var in natural jumped lab certain bioweapon ///
cost sue spending research suptrump supfed supcdc ///
supstate suplocal supmedia steps_1 steps_2 steps_3 ///
steps_4 steps_5 steps_6 steps_7 origin ///
personalsteps govsteps costsue{

qui reg `var' i.condition
eststo `var'm1

// exclude those missing state location
qui reg `var' i.condition if nostate==0
eststo `var'm2

// exclude those missing state location
// and those who were coded as a bad ip
qui reg `var' i.condition if nostate==0 & blockdummy==0
eststo `var'm3

// exclude those missing state location
// and those who were coded as a bad ip
// and those who spent less than 2 minutes
// or greater than 25 minutes on the survey
qui reg `var' i.condition if nostate==0 & blockdummy==0 & suspectshorttime==0 & suspectlongtime==0
eststo `var'm4

esttab `var'm1 `var'm2 `var'm3 `var'm4 , html  ///
cells((b(f(2) l(Coef.) star) p(f(3) l(p-value))) se(f(2) l(Std Err) par)) ///
label ///
nobaselevels ///
coeflabel(_cons "Constant (Control)") ///
mtitles("Main" "NoState" "NoState&IP" "NoState&IP&Time") ///
collabels("Coef." "p-value") ///
legend ///
modelwidth(10) ///
starlevel(* .10 ** .05 *** .01) ///
title("`: var label `var''" - Quality Checks) ///
note(Note: Cell entries are OLS coefficients. Standard errors ///
are in parentheses. Two-tailed p-values are reported. Model 1 is simple main effects. ///
Model 2 drops respondents missing state location data. Model 3 drops ///
respondents missing state location data and those with a flagged IP ///
address. Model 4 further drops respondents with less than 2 minutes ///
or greater than 25 minutes on survey time.) ///
aic bic

coefplot ///
`var'm1, bylabel(Model 1)  || ///
`var'm2, bylabel(Model 2) || ///
`var'm3, bylabel(Model 3) || ///
`var'm4, bylabel(Model 4) || ///
, drop(_cons)  ///
byopts(xrescale col(2) ///
title("`: var label `var'' - Quality Checks" , size(small))) ///
mlabel("{it:b} = " + string(@b, "%9.2f") + " , {it:p} = " + string(@pval,"%9.2f")) mlabpos(12) mlabgap(*4) ///
ciopts(recast(rcap) msize(large) lwidth(0.4)) ///
xline(0) scheme(plotplain)

graph export `var'qualitychecks.svg, width(4000)
graph export `var'qualitychecks.pdf
graph export `var'qualitychecks.png , width(10000)
}
```

Create Figures

```stata
cd "D:\Justin\Research\Other Projects\CoronavirusMTurk\New_US_Sample_New_Treatments\May4th2020\May 5th 2020\New Data\images"

foreach var in natural jumped lab certain bioweapon ///
cost sue spending research suptrump supfed supcdc ///
supstate suplocal supmedia steps_1 steps_2 steps_3 ///
steps_4 steps_5 steps_6 steps_7 origin ///
personalsteps govsteps costsue{
coefplot ///
`var'm1, bylabel(Model 1)  || ///
`var'm2, bylabel(Model 2) || ///
`var'm3, bylabel(Model 3) || ///
`var'm4, bylabel(Model 4) || ///
, drop(_cons)  ///
byopts(xrescale col(2) ///
title("`: var label `var'' - Quality Checks" , size(small))) ///
mlabel("{it:b} = " + string(@b, "%9.2f") + " , {it:p} = " + string(@pval,"%9.2f")) mlabpos(12) mlabgap(*4) ///
ciopts(recast(rcap) msize(large) lwidth(0.4)) ///
xline(0) scheme(plotplain)

graph export `var'qualitychecks.svg , width(4000)
graph export `var'qualitychecks.pdf
graph export `var'qualitychecks.png , width(10000)
}
```
