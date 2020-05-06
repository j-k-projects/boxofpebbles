---
toc: true
layout: post
description: A replication file with code and figures.
categories: [stata]
title: Analysis of New Data
---

# Analysis and Results

This notebook contains the data and code required to create the nessessary measures, estimate models, create (almost) publication ready tables, and produce the main effects figures.


```stata
// load the data
use Final_Data_May6th.dta
```

## Estimate Models

Our three dependent variables are `origin bioweapon` and `personalsteps`. Our variable `origin` is a measure of origin beliefs constructed using three measures of respondents' beliefs about whether or not the virus was a natural occurance that was transmitted from animals to humans. Our variable `personal steps` is a scaled measure capturing the extent to which the respondent supports, or finds nessessary, the following of three personal (pro-social) actions. Finally, the `bioweapon` variable is a measure of whether or not the respondent believes the virus occurs naturally in animals versus the virus being engineered as a bioweapon in a labaratory.

### A Nicer Version for Web Viewing

First, we use a loop to create all of the tables in a single batch. Then, we make them one at a time to make viewing them a little easier. 

#### A Batch of Tables


```stata
%html
foreach var in bioweapon origin personalsteps{

    qui reg `var' i.condition 
    eststo `var'

    esttab `var'   ,  ///
        cells((b(f(2) l(Coef.) star) p(f(3) l(p-value))) se(f(2) l(Std Err) par)) ///
        label ///
        nobaselevels ///
        coeflabel(_cons "Constant (Control)") ///
        nomtitle ///
        nonum ///
        collabels("Coefficient" "p-value") ///
        legend ///
        starlevel(* .10 ** .05 *** .01) ///
        title("`: var label `var''" - Main Effects) ///
        note(Note: Cell entries are OLS coefficients. Standard errors ///
        are in parentheses. Two-tailed p-values are reported.) ///
        aic bic html alignment(l) width(60%)
}
```




<table border="0" width="60%">
<caption>Bioweapon Created by Chinese Government - Main Effects</caption>
<tr><td colspan=3><hr></td></tr>
<tr><td>                    </td><td align="l"> Coefficient   </td><td align="l">     p-value</td></tr>
<tr><td colspan=3><hr></td></tr>
<tr><td>Natural Origin      </td><td align="l">       -0.23   </td><td align="l">       0.132</td></tr>
<tr><td>                    </td><td align="l">      (0.15)   </td><td align="l">            </td></tr>
<tr><td>Chinese Conspiracy  </td><td align="l">        0.38** </td><td align="l">       0.015</td></tr>
<tr><td>                    </td><td align="l">      (0.15)   </td><td align="l">            </td></tr>
<tr><td>Competitive Frame   </td><td align="l">        0.16   </td><td align="l">       0.294</td></tr>
<tr><td>                    </td><td align="l">      (0.15)   </td><td align="l">            </td></tr>
<tr><td>Constant (Control)  </td><td align="l">        3.40***</td><td align="l">       0.000</td></tr>
<tr><td>                    </td><td align="l">      (0.11)   </td><td align="l">            </td></tr>
<tr><td colspan=3><hr></td></tr>
<tr><td>Observations        </td><td align="l">        1071   </td><td align="l">            </td></tr>
<tr><td><i>AIC</i>          </td><td align="l">      4291.3   </td><td align="l">            </td></tr>
<tr><td><i>BIC</i>          </td><td align="l">      4311.3   </td><td align="l">            </td></tr>
<tr><td colspan=3><hr></td></tr>
<tr><td colspan=3>
Note: Cell entries are OLS coefficients. Standard errors are in parentheses. Two-tailed p-values are reported.
<br />* p<.10, ** p<.05, *** p<.01
</td></tr>
</table>

<table border="0" width="60%">
<caption>Origin Belief Scale - Main Effects</caption>
<tr><td colspan=3><hr></td></tr>
<tr><td>                    </td><td align="l"> Coefficient   </td><td align="l">     p-value</td></tr>
<tr><td colspan=3><hr></td></tr>
<tr><td>Natural Origin      </td><td align="l">       -0.36***</td><td align="l">       0.007</td></tr>
<tr><td>                    </td><td align="l">      (0.13)   </td><td align="l">            </td></tr>
<tr><td>Chinese Conspiracy  </td><td align="l">        0.40***</td><td align="l">       0.003</td></tr>
<tr><td>                    </td><td align="l">      (0.14)   </td><td align="l">            </td></tr>
<tr><td>Competitive Frame   </td><td align="l">        0.26*  </td><td align="l">       0.053</td></tr>
<tr><td>                    </td><td align="l">      (0.14)   </td><td align="l">            </td></tr>
<tr><td>Constant (Control)  </td><td align="l">        0.79***</td><td align="l">       0.000</td></tr>
<tr><td>                    </td><td align="l">      (0.10)   </td><td align="l">            </td></tr>
<tr><td colspan=3><hr></td></tr>
<tr><td>Observations        </td><td align="l">        1071   </td><td align="l">            </td></tr>
<tr><td><i>AIC</i>          </td><td align="l">      3999.9   </td><td align="l">            </td></tr>
<tr><td><i>BIC</i>          </td><td align="l">      4019.8   </td><td align="l">            </td></tr>
<tr><td colspan=3><hr></td></tr>
<tr><td colspan=3>
Note: Cell entries are OLS coefficients. Standard errors are in parentheses. Two-tailed p-values are reported.
<br />* p<.10, ** p<.05, *** p<.01
</td></tr>
</table>

<table border="0" width="60%">
<caption>Personal Steps Scale - Main Effects</caption>
<tr><td colspan=3><hr></td></tr>
<tr><td>                    </td><td align="l"> Coefficient   </td><td align="l">     p-value</td></tr>
<tr><td colspan=3><hr></td></tr>
<tr><td>Natural Origin      </td><td align="l">       -0.16   </td><td align="l">       0.121</td></tr>
<tr><td>                    </td><td align="l">      (0.10)   </td><td align="l">            </td></tr>
<tr><td>Chinese Conspiracy  </td><td align="l">       -0.26** </td><td align="l">       0.011</td></tr>
<tr><td>                    </td><td align="l">      (0.10)   </td><td align="l">            </td></tr>
<tr><td>Competitive Frame   </td><td align="l">       -0.21** </td><td align="l">       0.041</td></tr>
<tr><td>                    </td><td align="l">      (0.10)   </td><td align="l">            </td></tr>
<tr><td>Constant (Control)  </td><td align="l">        6.10***</td><td align="l">       0.000</td></tr>
<tr><td>                    </td><td align="l">      (0.07)   </td><td align="l">            </td></tr>
<tr><td colspan=3><hr></td></tr>
<tr><td>Observations        </td><td align="l">        1071   </td><td align="l">            </td></tr>
<tr><td><i>AIC</i>          </td><td align="l">      3408.8   </td><td align="l">            </td></tr>
<tr><td><i>BIC</i>          </td><td align="l">      3428.7   </td><td align="l">            </td></tr>
<tr><td colspan=3><hr></td></tr>
<tr><td colspan=3>
Note: Cell entries are OLS coefficients. Standard errors are in parentheses. Two-tailed p-values are reported.
<br />* p<.10, ** p<.05, *** p<.01
</td></tr>
</table>




#### Origin Beliefs


```stata
%html
foreach var in origin{

    qui reg `var' i.condition 
    eststo `var'

    esttab `var'   ,  ///
        cells((b(f(2) l(Coef.) star) p(f(3) l(p-value))) se(f(2) l(Std Err) par)) ///
        label ///
        nobaselevels ///
        coeflabel(_cons "Constant (Control)") ///
        nomtitle ///
        nonum ///
        collabels("Coefficient" "p-value") ///
        legend ///
        starlevel(* .10 ** .05 *** .01) ///
        title("`: var label `var''" - Main Effects) ///
        note(Note: Cell entries are OLS coefficients. Standard errors ///
        are in parentheses. Two-tailed p-values are reported.) ///
        aic bic html alignment(l) width(60%)
}
```




<table border="0" width="60%">
<caption>Origin Belief Scale - Main Effects</caption>
<tr><td colspan=3><hr></td></tr>
<tr><td>                    </td><td align="l"> Coefficient   </td><td align="l">     p-value</td></tr>
<tr><td colspan=3><hr></td></tr>
<tr><td>Natural Origin      </td><td align="l">       -0.36***</td><td align="l">       0.007</td></tr>
<tr><td>                    </td><td align="l">      (0.13)   </td><td align="l">            </td></tr>
<tr><td>Chinese Conspiracy  </td><td align="l">        0.40***</td><td align="l">       0.003</td></tr>
<tr><td>                    </td><td align="l">      (0.14)   </td><td align="l">            </td></tr>
<tr><td>Competitive Frame   </td><td align="l">        0.26*  </td><td align="l">       0.053</td></tr>
<tr><td>                    </td><td align="l">      (0.14)   </td><td align="l">            </td></tr>
<tr><td>Constant (Control)  </td><td align="l">        0.79***</td><td align="l">       0.000</td></tr>
<tr><td>                    </td><td align="l">      (0.10)   </td><td align="l">            </td></tr>
<tr><td colspan=3><hr></td></tr>
<tr><td>Observations        </td><td align="l">        1071   </td><td align="l">            </td></tr>
<tr><td><i>AIC</i>          </td><td align="l">      3999.9   </td><td align="l">            </td></tr>
<tr><td><i>BIC</i>          </td><td align="l">      4019.8   </td><td align="l">            </td></tr>
<tr><td colspan=3><hr></td></tr>
<tr><td colspan=3>
Note: Cell entries are OLS coefficients. Standard errors are in parentheses. Two-tailed p-values are reported.
<br />* p<.10, ** p<.05, *** p<.01
</td></tr>
</table>




#### Bioweapon


```stata
%html
foreach var in bioweapon{

    qui reg `var' i.condition 
    eststo `var'

    esttab `var'   ,  ///
        cells((b(f(2) l(Coef.) star) p(f(3) l(p-value))) se(f(2) l(Std Err) par)) ///
        label ///
        nobaselevels ///
        coeflabel(_cons "Constant (Control)") ///
        nomtitle ///
        nonum ///
        collabels("Coefficient" "p-value") ///
        legend ///
        starlevel(* .10 ** .05 *** .01) ///
        title("`: var label `var''" - Main Effects) ///
        note(Note: Cell entries are OLS coefficients. Standard errors ///
        are in parentheses. Two-tailed p-values are reported.) ///
        aic bic html alignment(l) width(60%)
}
```




<table border="0" width="60%">
<caption>Bioweapon Created by Chinese Government - Main Effects</caption>
<tr><td colspan=3><hr></td></tr>
<tr><td>                    </td><td align="l"> Coefficient   </td><td align="l">     p-value</td></tr>
<tr><td colspan=3><hr></td></tr>
<tr><td>Natural Origin      </td><td align="l">       -0.23   </td><td align="l">       0.132</td></tr>
<tr><td>                    </td><td align="l">      (0.15)   </td><td align="l">            </td></tr>
<tr><td>Chinese Conspiracy  </td><td align="l">        0.38** </td><td align="l">       0.015</td></tr>
<tr><td>                    </td><td align="l">      (0.15)   </td><td align="l">            </td></tr>
<tr><td>Competitive Frame   </td><td align="l">        0.16   </td><td align="l">       0.294</td></tr>
<tr><td>                    </td><td align="l">      (0.15)   </td><td align="l">            </td></tr>
<tr><td>Constant (Control)  </td><td align="l">        3.40***</td><td align="l">       0.000</td></tr>
<tr><td>                    </td><td align="l">      (0.11)   </td><td align="l">            </td></tr>
<tr><td colspan=3><hr></td></tr>
<tr><td>Observations        </td><td align="l">        1071   </td><td align="l">            </td></tr>
<tr><td><i>AIC</i>          </td><td align="l">      4291.3   </td><td align="l">            </td></tr>
<tr><td><i>BIC</i>          </td><td align="l">      4311.3   </td><td align="l">            </td></tr>
<tr><td colspan=3><hr></td></tr>
<tr><td colspan=3>
Note: Cell entries are OLS coefficients. Standard errors are in parentheses. Two-tailed p-values are reported.
<br />* p<.10, ** p<.05, *** p<.01
</td></tr>
</table>




#### Pro-Social Action


```stata
%html
foreach var in personalsteps{

    qui reg `var' i.condition 
    eststo `var'

    esttab `var'   ,  ///
        cells((b(f(2) l(Coef.) star) p(f(3) l(p-value))) se(f(2) l(Std Err) par)) ///
        label ///
        nobaselevels ///
        coeflabel(_cons "Constant (Control)") ///
        nomtitle ///
        nonum ///
        collabels("Coefficient" "p-value") ///
        legend ///
        starlevel(* .10 ** .05 *** .01) ///
        title(Pro-Social Actions - Main Effects) ///
        note(Note: Cell entries are OLS coefficients. Standard errors ///
        are in parentheses. Two-tailed p-values are reported.) ///
        aic bic html alignment(l) width(60%)
}
```




<table border="0" width="60%">
<caption>Pro-Social Actions - Main Effects</caption>
<tr><td colspan=3><hr></td></tr>
<tr><td>                    </td><td align="l"> Coefficient   </td><td align="l">     p-value</td></tr>
<tr><td colspan=3><hr></td></tr>
<tr><td>Natural Origin      </td><td align="l">       -0.16   </td><td align="l">       0.121</td></tr>
<tr><td>                    </td><td align="l">      (0.10)   </td><td align="l">            </td></tr>
<tr><td>Chinese Conspiracy  </td><td align="l">       -0.26** </td><td align="l">       0.011</td></tr>
<tr><td>                    </td><td align="l">      (0.10)   </td><td align="l">            </td></tr>
<tr><td>Competitive Frame   </td><td align="l">       -0.21** </td><td align="l">       0.041</td></tr>
<tr><td>                    </td><td align="l">      (0.10)   </td><td align="l">            </td></tr>
<tr><td>Constant (Control)  </td><td align="l">        6.10***</td><td align="l">       0.000</td></tr>
<tr><td>                    </td><td align="l">      (0.07)   </td><td align="l">            </td></tr>
<tr><td colspan=3><hr></td></tr>
<tr><td>Observations        </td><td align="l">        1071   </td><td align="l">            </td></tr>
<tr><td><i>AIC</i>          </td><td align="l">      3408.8   </td><td align="l">            </td></tr>
<tr><td><i>BIC</i>          </td><td align="l">      3428.7   </td><td align="l">            </td></tr>
<tr><td colspan=3><hr></td></tr>
<tr><td colspan=3>
Note: Cell entries are OLS coefficients. Standard errors are in parentheses. Two-tailed p-values are reported.
<br />* p<.10, ** p<.05, *** p<.01
</td></tr>
</table>




### A Word Document for Tables

Running the code below will create a new RTF document that can be opened directly with Word.

```stata
foreach var in bioweapon origin personalsteps{

    qui reg `var' i.condition 
    eststo `var'

    esttab `var' using A_File_Name.rtf  , append  ///
    cells((b(f(2) l(Coef.) star) p(f(3) l(p-value))) se(f(2) l(Std Err) par)) ///
    label ///
    nobaselevels ///
    coeflabel(_cons "Constant (Control)") ///
    nomtitle ///
    nonum ///
    collabels("Coefficient" "p-value") ///
    legend ///
    modelwidth(12) ///
    starlevel(* .10 ** .05 *** .01) ///
    title("`: var label `var''" - Main Effects) ///
    note(Note: Cell entries are OLS coefficients. Standard errors ///
    are in parentheses. Two-tailed p-values are reported.) ///
    aic bic 
}
```


```stata
mkdir tablesandfigures
```


```stata
foreach var in bioweapon origin personalsteps{

    qui reg `var' i.condition 
    eststo `var'

    esttab `var' using "tablesandfigures\A_File_Name.rtf"  , append  ///
    cells((b(f(2) l(Coef.) star) p(f(3) l(p-value))) se(f(2) l(Std Err) par)) ///
    label ///
    nobaselevels ///
    coeflabel(_cons "Constant (Control)") ///
    nomtitle ///
    nonum ///
    collabels("Coefficient" "p-value") ///
    legend ///
    modelwidth(12) ///
    starlevel(* .10 ** .05 *** .01) ///
    title("`: var label `var''" - Main Effects) ///
    note(Note: Cell entries are OLS coefficients. Standard errors ///
    are in parentheses. Two-tailed p-values are reported.) ///
    aic bic 
}
```

    
    (note: file tablesandfigures\A_File_Name.rtf not found)
    (output written to tablesandfigures\A_File_Name.rtf)
    (output written to tablesandfigures\A_File_Name.rtf)
    (output written to tablesandfigures\A_File_Name.rtf)
    

## Plotting Coefficients

The code below creates a figure plotting the coefficient estimates for the main effects models. 


```stata
cd tablesandfigures

coefplot origin , bylabel(Origin Beliefs) || ///
bioweapon , bylabel(Bioweapon) || ///
personalsteps , bylabel(Pro-Social Behavior) || ///
, drop(_cons) ///
byopts(xrescale col(3)) ///
mlabel("{it:b} = " + string(@b, "%9.2f") + ", {it:p} = " + string(@pval,"%9.2f")) mlabpos(12) mlabgap(*4) ///
ciopts(recast(rcap) msize(large) lwidth(0.4)) ///
xline(0) scheme(plotplain) ///
saving(Main_Effects.gph)
graph export Main_Effects.svg , width(10000) 
graph export Main_Effects.pdf 
graph export Main_Effects.png , width(10000)
```

    
    D:\Justin\Research\Other Projects\CoronavirusMTurk\New_US_Sample_New_Treatments\
    > May6th2020\Analysis and Data May 6th 2020\Notebook\tablesandfigures
    
    (file Main_Effects.gph saved)
    


                <iframe frameborder="0" scrolling="no" height="400" width="600"                srcdoc="<html><body>&lt;?xml version=&quot;1.0&quot; encoding=&quot;UTF-8&quot; standalone=&quot;no&quot;?&gt;
&lt;!-- This is a Stata 15.1 generated SVG file (http://www.stata.com) --&gt;

&lt;svg version=&quot;1.1&quot; width=&quot;600px&quot; height=&quot;400px&quot; viewBox=&quot;0 0 4320 2880&quot; xmlns=&quot;http://www.w3.org/2000/svg&quot; xmlns:xlink=&quot;http://www.w3.org/1999/xlink&quot;&gt;
	&lt;desc&gt;Stata Graph - Graph&lt;/desc&gt;
	&lt;rect x=&quot;0&quot; y=&quot;0&quot; width=&quot;4320&quot; height=&quot;2880&quot; style=&quot;fill:#FFFFFF;stroke:none&quot;/&gt;
	&lt;rect x=&quot;0.00&quot; y=&quot;0.00&quot; width=&quot;4320.00&quot; height=&quot;2880.00&quot; style=&quot;fill:#FFFFFF&quot;/&gt;
	&lt;rect x=&quot;2.88&quot; y=&quot;2.88&quot; width=&quot;4314.24&quot; height=&quot;2874.24&quot; style=&quot;fill:none;stroke:#FFFFFF;stroke-width:5.76&quot;/&gt;
	&lt;line x1=&quot;770.45&quot; y1=&quot;642.88&quot; x2=&quot;773.28&quot; y2=&quot;642.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;793.53&quot; y1=&quot;642.88&quot; x2=&quot;796.37&quot; y2=&quot;642.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;816.48&quot; y1=&quot;642.88&quot; x2=&quot;819.45&quot; y2=&quot;642.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;839.57&quot; y1=&quot;642.88&quot; x2=&quot;842.40&quot; y2=&quot;642.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;862.65&quot; y1=&quot;642.88&quot; x2=&quot;865.49&quot; y2=&quot;642.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;885.74&quot; y1=&quot;642.88&quot; x2=&quot;888.57&quot; y2=&quot;642.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;908.82&quot; y1=&quot;642.88&quot; x2=&quot;911.65&quot; y2=&quot;642.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;931.90&quot; y1=&quot;642.88&quot; x2=&quot;934.74&quot; y2=&quot;642.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;954.86&quot; y1=&quot;642.88&quot; x2=&quot;957.83&quot; y2=&quot;642.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;977.94&quot; y1=&quot;642.88&quot; x2=&quot;980.77&quot; y2=&quot;642.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1001.02&quot; y1=&quot;642.88&quot; x2=&quot;1003.86&quot; y2=&quot;642.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1024.11&quot; y1=&quot;642.88&quot; x2=&quot;1026.94&quot; y2=&quot;642.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1047.19&quot; y1=&quot;642.88&quot; x2=&quot;1050.03&quot; y2=&quot;642.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1070.28&quot; y1=&quot;642.88&quot; x2=&quot;1073.12&quot; y2=&quot;642.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1093.23&quot; y1=&quot;642.88&quot; x2=&quot;1096.20&quot; y2=&quot;642.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1116.32&quot; y1=&quot;642.88&quot; x2=&quot;1119.15&quot; y2=&quot;642.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1139.40&quot; y1=&quot;642.88&quot; x2=&quot;1142.23&quot; y2=&quot;642.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1162.48&quot; y1=&quot;642.88&quot; x2=&quot;1165.32&quot; y2=&quot;642.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1185.57&quot; y1=&quot;642.88&quot; x2=&quot;1188.40&quot; y2=&quot;642.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1208.65&quot; y1=&quot;642.88&quot; x2=&quot;1211.49&quot; y2=&quot;642.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1231.61&quot; y1=&quot;642.88&quot; x2=&quot;1234.58&quot; y2=&quot;642.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1254.69&quot; y1=&quot;642.88&quot; x2=&quot;1257.53&quot; y2=&quot;642.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1277.78&quot; y1=&quot;642.88&quot; x2=&quot;1280.61&quot; y2=&quot;642.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1300.86&quot; y1=&quot;642.88&quot; x2=&quot;1303.69&quot; y2=&quot;642.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1323.94&quot; y1=&quot;642.88&quot; x2=&quot;1326.78&quot; y2=&quot;642.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1347.03&quot; y1=&quot;642.88&quot; x2=&quot;1349.87&quot; y2=&quot;642.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1369.98&quot; y1=&quot;642.88&quot; x2=&quot;1372.95&quot; y2=&quot;642.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1393.07&quot; y1=&quot;642.88&quot; x2=&quot;1395.90&quot; y2=&quot;642.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1416.15&quot; y1=&quot;642.88&quot; x2=&quot;1418.98&quot; y2=&quot;642.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1439.23&quot; y1=&quot;642.88&quot; x2=&quot;1442.07&quot; y2=&quot;642.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1462.32&quot; y1=&quot;642.88&quot; x2=&quot;1465.15&quot; y2=&quot;642.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1485.40&quot; y1=&quot;642.88&quot; x2=&quot;1488.24&quot; y2=&quot;642.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1508.36&quot; y1=&quot;642.88&quot; x2=&quot;1511.33&quot; y2=&quot;642.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1531.44&quot; y1=&quot;642.88&quot; x2=&quot;1534.28&quot; y2=&quot;642.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1554.53&quot; y1=&quot;642.88&quot; x2=&quot;1557.36&quot; y2=&quot;642.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1577.61&quot; y1=&quot;642.88&quot; x2=&quot;1580.44&quot; y2=&quot;642.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1600.69&quot; y1=&quot;642.88&quot; x2=&quot;1603.53&quot; y2=&quot;642.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1623.78&quot; y1=&quot;642.88&quot; x2=&quot;1626.62&quot; y2=&quot;642.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1646.73&quot; y1=&quot;642.88&quot; x2=&quot;1649.70&quot; y2=&quot;642.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1669.82&quot; y1=&quot;642.88&quot; x2=&quot;1672.65&quot; y2=&quot;642.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1692.90&quot; y1=&quot;642.88&quot; x2=&quot;1695.73&quot; y2=&quot;642.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1715.98&quot; y1=&quot;642.88&quot; x2=&quot;1718.82&quot; y2=&quot;642.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1739.07&quot; y1=&quot;642.88&quot; x2=&quot;1741.90&quot; y2=&quot;642.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1762.15&quot; y1=&quot;642.88&quot; x2=&quot;1764.99&quot; y2=&quot;642.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1785.11&quot; y1=&quot;642.88&quot; x2=&quot;1788.08&quot; y2=&quot;642.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1808.19&quot; y1=&quot;642.88&quot; x2=&quot;1811.03&quot; y2=&quot;642.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1831.28&quot; y1=&quot;642.88&quot; x2=&quot;1834.11&quot; y2=&quot;642.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1854.36&quot; y1=&quot;642.88&quot; x2=&quot;1857.19&quot; y2=&quot;642.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;770.45&quot; y1=&quot;1429.13&quot; x2=&quot;773.28&quot; y2=&quot;1429.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;793.53&quot; y1=&quot;1429.13&quot; x2=&quot;796.37&quot; y2=&quot;1429.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;816.48&quot; y1=&quot;1429.13&quot; x2=&quot;819.45&quot; y2=&quot;1429.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;839.57&quot; y1=&quot;1429.13&quot; x2=&quot;842.40&quot; y2=&quot;1429.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;862.65&quot; y1=&quot;1429.13&quot; x2=&quot;865.49&quot; y2=&quot;1429.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;885.74&quot; y1=&quot;1429.13&quot; x2=&quot;888.57&quot; y2=&quot;1429.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;908.82&quot; y1=&quot;1429.13&quot; x2=&quot;911.65&quot; y2=&quot;1429.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;931.90&quot; y1=&quot;1429.13&quot; x2=&quot;934.74&quot; y2=&quot;1429.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;954.86&quot; y1=&quot;1429.13&quot; x2=&quot;957.83&quot; y2=&quot;1429.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;977.94&quot; y1=&quot;1429.13&quot; x2=&quot;980.77&quot; y2=&quot;1429.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1001.02&quot; y1=&quot;1429.13&quot; x2=&quot;1003.86&quot; y2=&quot;1429.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1024.11&quot; y1=&quot;1429.13&quot; x2=&quot;1026.94&quot; y2=&quot;1429.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1047.19&quot; y1=&quot;1429.13&quot; x2=&quot;1050.03&quot; y2=&quot;1429.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1070.28&quot; y1=&quot;1429.13&quot; x2=&quot;1073.12&quot; y2=&quot;1429.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1093.23&quot; y1=&quot;1429.13&quot; x2=&quot;1096.20&quot; y2=&quot;1429.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1116.32&quot; y1=&quot;1429.13&quot; x2=&quot;1119.15&quot; y2=&quot;1429.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1139.40&quot; y1=&quot;1429.13&quot; x2=&quot;1142.23&quot; y2=&quot;1429.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1162.48&quot; y1=&quot;1429.13&quot; x2=&quot;1165.32&quot; y2=&quot;1429.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1185.57&quot; y1=&quot;1429.13&quot; x2=&quot;1188.40&quot; y2=&quot;1429.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1208.65&quot; y1=&quot;1429.13&quot; x2=&quot;1211.49&quot; y2=&quot;1429.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1231.61&quot; y1=&quot;1429.13&quot; x2=&quot;1234.58&quot; y2=&quot;1429.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1254.69&quot; y1=&quot;1429.13&quot; x2=&quot;1257.53&quot; y2=&quot;1429.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1277.78&quot; y1=&quot;1429.13&quot; x2=&quot;1280.61&quot; y2=&quot;1429.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1300.86&quot; y1=&quot;1429.13&quot; x2=&quot;1303.69&quot; y2=&quot;1429.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1323.94&quot; y1=&quot;1429.13&quot; x2=&quot;1326.78&quot; y2=&quot;1429.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1347.03&quot; y1=&quot;1429.13&quot; x2=&quot;1349.87&quot; y2=&quot;1429.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1369.98&quot; y1=&quot;1429.13&quot; x2=&quot;1372.95&quot; y2=&quot;1429.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1393.07&quot; y1=&quot;1429.13&quot; x2=&quot;1395.90&quot; y2=&quot;1429.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1416.15&quot; y1=&quot;1429.13&quot; x2=&quot;1418.98&quot; y2=&quot;1429.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1439.23&quot; y1=&quot;1429.13&quot; x2=&quot;1442.07&quot; y2=&quot;1429.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1462.32&quot; y1=&quot;1429.13&quot; x2=&quot;1465.15&quot; y2=&quot;1429.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1485.40&quot; y1=&quot;1429.13&quot; x2=&quot;1488.24&quot; y2=&quot;1429.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1508.36&quot; y1=&quot;1429.13&quot; x2=&quot;1511.33&quot; y2=&quot;1429.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1531.44&quot; y1=&quot;1429.13&quot; x2=&quot;1534.28&quot; y2=&quot;1429.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1554.53&quot; y1=&quot;1429.13&quot; x2=&quot;1557.36&quot; y2=&quot;1429.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1577.61&quot; y1=&quot;1429.13&quot; x2=&quot;1580.44&quot; y2=&quot;1429.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1600.69&quot; y1=&quot;1429.13&quot; x2=&quot;1603.53&quot; y2=&quot;1429.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1623.78&quot; y1=&quot;1429.13&quot; x2=&quot;1626.62&quot; y2=&quot;1429.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1646.73&quot; y1=&quot;1429.13&quot; x2=&quot;1649.70&quot; y2=&quot;1429.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1669.82&quot; y1=&quot;1429.13&quot; x2=&quot;1672.65&quot; y2=&quot;1429.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1692.90&quot; y1=&quot;1429.13&quot; x2=&quot;1695.73&quot; y2=&quot;1429.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1715.98&quot; y1=&quot;1429.13&quot; x2=&quot;1718.82&quot; y2=&quot;1429.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1739.07&quot; y1=&quot;1429.13&quot; x2=&quot;1741.90&quot; y2=&quot;1429.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1762.15&quot; y1=&quot;1429.13&quot; x2=&quot;1764.99&quot; y2=&quot;1429.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1785.11&quot; y1=&quot;1429.13&quot; x2=&quot;1788.08&quot; y2=&quot;1429.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1808.19&quot; y1=&quot;1429.13&quot; x2=&quot;1811.03&quot; y2=&quot;1429.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1831.28&quot; y1=&quot;1429.13&quot; x2=&quot;1834.11&quot; y2=&quot;1429.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1854.36&quot; y1=&quot;1429.13&quot; x2=&quot;1857.19&quot; y2=&quot;1429.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;770.45&quot; y1=&quot;2215.52&quot; x2=&quot;773.28&quot; y2=&quot;2215.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;793.53&quot; y1=&quot;2215.52&quot; x2=&quot;796.37&quot; y2=&quot;2215.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;816.48&quot; y1=&quot;2215.52&quot; x2=&quot;819.45&quot; y2=&quot;2215.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;839.57&quot; y1=&quot;2215.52&quot; x2=&quot;842.40&quot; y2=&quot;2215.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;862.65&quot; y1=&quot;2215.52&quot; x2=&quot;865.49&quot; y2=&quot;2215.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;885.74&quot; y1=&quot;2215.52&quot; x2=&quot;888.57&quot; y2=&quot;2215.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;908.82&quot; y1=&quot;2215.52&quot; x2=&quot;911.65&quot; y2=&quot;2215.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;931.90&quot; y1=&quot;2215.52&quot; x2=&quot;934.74&quot; y2=&quot;2215.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;954.86&quot; y1=&quot;2215.52&quot; x2=&quot;957.83&quot; y2=&quot;2215.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;977.94&quot; y1=&quot;2215.52&quot; x2=&quot;980.77&quot; y2=&quot;2215.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1001.02&quot; y1=&quot;2215.52&quot; x2=&quot;1003.86&quot; y2=&quot;2215.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1024.11&quot; y1=&quot;2215.52&quot; x2=&quot;1026.94&quot; y2=&quot;2215.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1047.19&quot; y1=&quot;2215.52&quot; x2=&quot;1050.03&quot; y2=&quot;2215.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1070.28&quot; y1=&quot;2215.52&quot; x2=&quot;1073.12&quot; y2=&quot;2215.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1093.23&quot; y1=&quot;2215.52&quot; x2=&quot;1096.20&quot; y2=&quot;2215.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1116.32&quot; y1=&quot;2215.52&quot; x2=&quot;1119.15&quot; y2=&quot;2215.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1139.40&quot; y1=&quot;2215.52&quot; x2=&quot;1142.23&quot; y2=&quot;2215.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1162.48&quot; y1=&quot;2215.52&quot; x2=&quot;1165.32&quot; y2=&quot;2215.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1185.57&quot; y1=&quot;2215.52&quot; x2=&quot;1188.40&quot; y2=&quot;2215.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1208.65&quot; y1=&quot;2215.52&quot; x2=&quot;1211.49&quot; y2=&quot;2215.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1231.61&quot; y1=&quot;2215.52&quot; x2=&quot;1234.58&quot; y2=&quot;2215.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1254.69&quot; y1=&quot;2215.52&quot; x2=&quot;1257.53&quot; y2=&quot;2215.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1277.78&quot; y1=&quot;2215.52&quot; x2=&quot;1280.61&quot; y2=&quot;2215.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1300.86&quot; y1=&quot;2215.52&quot; x2=&quot;1303.69&quot; y2=&quot;2215.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1323.94&quot; y1=&quot;2215.52&quot; x2=&quot;1326.78&quot; y2=&quot;2215.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1347.03&quot; y1=&quot;2215.52&quot; x2=&quot;1349.87&quot; y2=&quot;2215.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1369.98&quot; y1=&quot;2215.52&quot; x2=&quot;1372.95&quot; y2=&quot;2215.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1393.07&quot; y1=&quot;2215.52&quot; x2=&quot;1395.90&quot; y2=&quot;2215.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1416.15&quot; y1=&quot;2215.52&quot; x2=&quot;1418.98&quot; y2=&quot;2215.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1439.23&quot; y1=&quot;2215.52&quot; x2=&quot;1442.07&quot; y2=&quot;2215.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1462.32&quot; y1=&quot;2215.52&quot; x2=&quot;1465.15&quot; y2=&quot;2215.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1485.40&quot; y1=&quot;2215.52&quot; x2=&quot;1488.24&quot; y2=&quot;2215.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1508.36&quot; y1=&quot;2215.52&quot; x2=&quot;1511.33&quot; y2=&quot;2215.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1531.44&quot; y1=&quot;2215.52&quot; x2=&quot;1534.28&quot; y2=&quot;2215.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1554.53&quot; y1=&quot;2215.52&quot; x2=&quot;1557.36&quot; y2=&quot;2215.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1577.61&quot; y1=&quot;2215.52&quot; x2=&quot;1580.44&quot; y2=&quot;2215.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1600.69&quot; y1=&quot;2215.52&quot; x2=&quot;1603.53&quot; y2=&quot;2215.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1623.78&quot; y1=&quot;2215.52&quot; x2=&quot;1626.62&quot; y2=&quot;2215.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1646.73&quot; y1=&quot;2215.52&quot; x2=&quot;1649.70&quot; y2=&quot;2215.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1669.82&quot; y1=&quot;2215.52&quot; x2=&quot;1672.65&quot; y2=&quot;2215.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1692.90&quot; y1=&quot;2215.52&quot; x2=&quot;1695.73&quot; y2=&quot;2215.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1715.98&quot; y1=&quot;2215.52&quot; x2=&quot;1718.82&quot; y2=&quot;2215.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1739.07&quot; y1=&quot;2215.52&quot; x2=&quot;1741.90&quot; y2=&quot;2215.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1762.15&quot; y1=&quot;2215.52&quot; x2=&quot;1764.99&quot; y2=&quot;2215.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1785.11&quot; y1=&quot;2215.52&quot; x2=&quot;1788.08&quot; y2=&quot;2215.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1808.19&quot; y1=&quot;2215.52&quot; x2=&quot;1811.03&quot; y2=&quot;2215.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1831.28&quot; y1=&quot;2215.52&quot; x2=&quot;1834.11&quot; y2=&quot;2215.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1854.36&quot; y1=&quot;2215.52&quot; x2=&quot;1857.19&quot; y2=&quot;2215.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;901.80&quot; y1=&quot;2608.65&quot; x2=&quot;901.80&quot; y2=&quot;2605.81&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;901.80&quot; y1=&quot;2585.56&quot; x2=&quot;901.80&quot; y2=&quot;2582.73&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;901.80&quot; y1=&quot;2562.48&quot; x2=&quot;901.80&quot; y2=&quot;2559.64&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;901.80&quot; y1=&quot;2539.39&quot; x2=&quot;901.80&quot; y2=&quot;2536.55&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;901.80&quot; y1=&quot;2516.44&quot; x2=&quot;901.80&quot; y2=&quot;2513.47&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;901.80&quot; y1=&quot;2493.35&quot; x2=&quot;901.80&quot; y2=&quot;2490.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;901.80&quot; y1=&quot;2470.27&quot; x2=&quot;901.80&quot; y2=&quot;2467.43&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;901.80&quot; y1=&quot;2447.18&quot; x2=&quot;901.80&quot; y2=&quot;2444.35&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;901.80&quot; y1=&quot;2424.10&quot; x2=&quot;901.80&quot; y2=&quot;2421.26&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;901.80&quot; y1=&quot;2401.01&quot; x2=&quot;901.80&quot; y2=&quot;2398.18&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;901.80&quot; y1=&quot;2378.06&quot; x2=&quot;901.80&quot; y2=&quot;2375.09&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;901.80&quot; y1=&quot;2354.98&quot; x2=&quot;901.80&quot; y2=&quot;2352.14&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;901.80&quot; y1=&quot;2331.89&quot; x2=&quot;901.80&quot; y2=&quot;2329.06&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;901.80&quot; y1=&quot;2308.81&quot; x2=&quot;901.80&quot; y2=&quot;2305.97&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;901.80&quot; y1=&quot;2285.72&quot; x2=&quot;901.80&quot; y2=&quot;2282.89&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;901.80&quot; y1=&quot;2262.64&quot; x2=&quot;901.80&quot; y2=&quot;2259.80&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;901.80&quot; y1=&quot;2239.68&quot; x2=&quot;901.80&quot; y2=&quot;2236.71&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;901.80&quot; y1=&quot;2216.60&quot; x2=&quot;901.80&quot; y2=&quot;2213.76&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;901.80&quot; y1=&quot;2193.51&quot; x2=&quot;901.80&quot; y2=&quot;2190.68&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;901.80&quot; y1=&quot;2170.43&quot; x2=&quot;901.80&quot; y2=&quot;2167.59&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;901.80&quot; y1=&quot;2147.34&quot; x2=&quot;901.80&quot; y2=&quot;2144.51&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;901.80&quot; y1=&quot;2124.26&quot; x2=&quot;901.80&quot; y2=&quot;2121.42&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;901.80&quot; y1=&quot;2101.31&quot; x2=&quot;901.80&quot; y2=&quot;2098.34&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;901.80&quot; y1=&quot;2078.22&quot; x2=&quot;901.80&quot; y2=&quot;2075.39&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;901.80&quot; y1=&quot;2055.14&quot; x2=&quot;901.80&quot; y2=&quot;2052.30&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;901.80&quot; y1=&quot;2032.05&quot; x2=&quot;901.80&quot; y2=&quot;2029.22&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;901.80&quot; y1=&quot;2008.97&quot; x2=&quot;901.80&quot; y2=&quot;2006.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;901.80&quot; y1=&quot;1985.88&quot; x2=&quot;901.80&quot; y2=&quot;1983.05&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;901.80&quot; y1=&quot;1962.93&quot; x2=&quot;901.80&quot; y2=&quot;1959.96&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;901.80&quot; y1=&quot;1939.85&quot; x2=&quot;901.80&quot; y2=&quot;1937.01&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;901.80&quot; y1=&quot;1916.76&quot; x2=&quot;901.80&quot; y2=&quot;1913.92&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;901.80&quot; y1=&quot;1893.67&quot; x2=&quot;901.80&quot; y2=&quot;1890.84&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;901.80&quot; y1=&quot;1870.59&quot; x2=&quot;901.80&quot; y2=&quot;1867.75&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;901.80&quot; y1=&quot;1847.50&quot; x2=&quot;901.80&quot; y2=&quot;1844.67&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;901.80&quot; y1=&quot;1824.55&quot; x2=&quot;901.80&quot; y2=&quot;1821.58&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;901.80&quot; y1=&quot;1801.47&quot; x2=&quot;901.80&quot; y2=&quot;1798.63&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;901.80&quot; y1=&quot;1778.38&quot; x2=&quot;901.80&quot; y2=&quot;1775.55&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;901.80&quot; y1=&quot;1755.30&quot; x2=&quot;901.80&quot; y2=&quot;1752.46&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;901.80&quot; y1=&quot;1732.21&quot; x2=&quot;901.80&quot; y2=&quot;1729.38&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;901.80&quot; y1=&quot;1709.26&quot; x2=&quot;901.80&quot; y2=&quot;1706.29&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;901.80&quot; y1=&quot;1686.18&quot; x2=&quot;901.80&quot; y2=&quot;1683.34&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;901.80&quot; y1=&quot;1663.09&quot; x2=&quot;901.80&quot; y2=&quot;1660.26&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;901.80&quot; y1=&quot;1640.01&quot; x2=&quot;901.80&quot; y2=&quot;1637.17&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;901.80&quot; y1=&quot;1616.92&quot; x2=&quot;901.80&quot; y2=&quot;1614.09&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;901.80&quot; y1=&quot;1593.83&quot; x2=&quot;901.80&quot; y2=&quot;1591.00&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;901.80&quot; y1=&quot;1570.88&quot; x2=&quot;901.80&quot; y2=&quot;1567.91&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;901.80&quot; y1=&quot;1547.80&quot; x2=&quot;901.80&quot; y2=&quot;1544.96&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;901.80&quot; y1=&quot;1524.71&quot; x2=&quot;901.80&quot; y2=&quot;1521.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;901.80&quot; y1=&quot;1501.63&quot; x2=&quot;901.80&quot; y2=&quot;1498.79&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;901.80&quot; y1=&quot;1478.54&quot; x2=&quot;901.80&quot; y2=&quot;1475.71&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;901.80&quot; y1=&quot;1455.46&quot; x2=&quot;901.80&quot; y2=&quot;1452.62&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;901.80&quot; y1=&quot;1432.51&quot; x2=&quot;901.80&quot; y2=&quot;1429.54&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;901.80&quot; y1=&quot;1409.42&quot; x2=&quot;901.80&quot; y2=&quot;1406.59&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;901.80&quot; y1=&quot;1386.34&quot; x2=&quot;901.80&quot; y2=&quot;1383.50&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;901.80&quot; y1=&quot;1363.25&quot; x2=&quot;901.80&quot; y2=&quot;1360.42&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;901.80&quot; y1=&quot;1340.17&quot; x2=&quot;901.80&quot; y2=&quot;1337.33&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;901.80&quot; y1=&quot;1317.08&quot; x2=&quot;901.80&quot; y2=&quot;1314.25&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;901.80&quot; y1=&quot;1294.13&quot; x2=&quot;901.80&quot; y2=&quot;1291.16&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;901.80&quot; y1=&quot;1271.04&quot; x2=&quot;901.80&quot; y2=&quot;1268.21&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;901.80&quot; y1=&quot;1247.96&quot; x2=&quot;901.80&quot; y2=&quot;1245.12&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;901.80&quot; y1=&quot;1224.87&quot; x2=&quot;901.80&quot; y2=&quot;1222.04&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;901.80&quot; y1=&quot;1201.79&quot; x2=&quot;901.80&quot; y2=&quot;1198.95&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;901.80&quot; y1=&quot;1178.70&quot; x2=&quot;901.80&quot; y2=&quot;1175.87&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;901.80&quot; y1=&quot;1155.75&quot; x2=&quot;901.80&quot; y2=&quot;1152.78&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;901.80&quot; y1=&quot;1132.67&quot; x2=&quot;901.80&quot; y2=&quot;1129.83&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;901.80&quot; y1=&quot;1109.58&quot; x2=&quot;901.80&quot; y2=&quot;1106.75&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;901.80&quot; y1=&quot;1086.50&quot; x2=&quot;901.80&quot; y2=&quot;1083.66&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;901.80&quot; y1=&quot;1063.41&quot; x2=&quot;901.80&quot; y2=&quot;1060.58&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;901.80&quot; y1=&quot;1040.33&quot; x2=&quot;901.80&quot; y2=&quot;1037.49&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;901.80&quot; y1=&quot;1017.38&quot; x2=&quot;901.80&quot; y2=&quot;1014.41&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;901.80&quot; y1=&quot;994.29&quot; x2=&quot;901.80&quot; y2=&quot;991.46&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;901.80&quot; y1=&quot;971.21&quot; x2=&quot;901.80&quot; y2=&quot;968.37&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;901.80&quot; y1=&quot;948.12&quot; x2=&quot;901.80&quot; y2=&quot;945.28&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;901.80&quot; y1=&quot;925.03&quot; x2=&quot;901.80&quot; y2=&quot;922.20&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;901.80&quot; y1=&quot;901.95&quot; x2=&quot;901.80&quot; y2=&quot;899.11&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;901.80&quot; y1=&quot;879.00&quot; x2=&quot;901.80&quot; y2=&quot;876.03&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;901.80&quot; y1=&quot;855.91&quot; x2=&quot;901.80&quot; y2=&quot;853.08&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;901.80&quot; y1=&quot;832.83&quot; x2=&quot;901.80&quot; y2=&quot;829.99&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;901.80&quot; y1=&quot;809.74&quot; x2=&quot;901.80&quot; y2=&quot;806.91&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;901.80&quot; y1=&quot;786.66&quot; x2=&quot;901.80&quot; y2=&quot;783.82&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;901.80&quot; y1=&quot;763.57&quot; x2=&quot;901.80&quot; y2=&quot;760.74&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;901.80&quot; y1=&quot;740.62&quot; x2=&quot;901.80&quot; y2=&quot;737.65&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;901.80&quot; y1=&quot;717.54&quot; x2=&quot;901.80&quot; y2=&quot;714.70&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;901.80&quot; y1=&quot;694.45&quot; x2=&quot;901.80&quot; y2=&quot;691.62&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;901.80&quot; y1=&quot;671.37&quot; x2=&quot;901.80&quot; y2=&quot;668.53&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;901.80&quot; y1=&quot;648.28&quot; x2=&quot;901.80&quot; y2=&quot;645.45&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;901.80&quot; y1=&quot;625.19&quot; x2=&quot;901.80&quot; y2=&quot;622.36&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;901.80&quot; y1=&quot;602.24&quot; x2=&quot;901.80&quot; y2=&quot;599.27&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;901.80&quot; y1=&quot;579.16&quot; x2=&quot;901.80&quot; y2=&quot;576.32&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;901.80&quot; y1=&quot;556.07&quot; x2=&quot;901.80&quot; y2=&quot;553.24&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;901.80&quot; y1=&quot;532.99&quot; x2=&quot;901.80&quot; y2=&quot;530.15&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;901.80&quot; y1=&quot;509.90&quot; x2=&quot;901.80&quot; y2=&quot;507.07&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;901.80&quot; y1=&quot;486.82&quot; x2=&quot;901.80&quot; y2=&quot;483.98&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;901.80&quot; y1=&quot;463.87&quot; x2=&quot;901.80&quot; y2=&quot;460.90&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;901.80&quot; y1=&quot;440.78&quot; x2=&quot;901.80&quot; y2=&quot;437.95&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;901.80&quot; y1=&quot;417.70&quot; x2=&quot;901.80&quot; y2=&quot;414.86&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;901.80&quot; y1=&quot;394.61&quot; x2=&quot;901.80&quot; y2=&quot;391.78&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;901.80&quot; y1=&quot;371.53&quot; x2=&quot;901.80&quot; y2=&quot;368.69&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;901.80&quot; y1=&quot;348.44&quot; x2=&quot;901.80&quot; y2=&quot;345.61&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;901.80&quot; y1=&quot;325.49&quot; x2=&quot;901.80&quot; y2=&quot;322.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;901.80&quot; y1=&quot;302.40&quot; x2=&quot;901.80&quot; y2=&quot;299.57&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;901.80&quot; y1=&quot;279.32&quot; x2=&quot;901.80&quot; y2=&quot;276.48&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;901.80&quot; y1=&quot;256.23&quot; x2=&quot;901.80&quot; y2=&quot;253.40&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;2608.65&quot; x2=&quot;1204.07&quot; y2=&quot;2605.81&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;2585.56&quot; x2=&quot;1204.07&quot; y2=&quot;2582.73&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;2562.48&quot; x2=&quot;1204.07&quot; y2=&quot;2559.64&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;2539.39&quot; x2=&quot;1204.07&quot; y2=&quot;2536.55&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;2516.44&quot; x2=&quot;1204.07&quot; y2=&quot;2513.47&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;2493.35&quot; x2=&quot;1204.07&quot; y2=&quot;2490.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;2470.27&quot; x2=&quot;1204.07&quot; y2=&quot;2467.43&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;2447.18&quot; x2=&quot;1204.07&quot; y2=&quot;2444.35&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;2424.10&quot; x2=&quot;1204.07&quot; y2=&quot;2421.26&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;2401.01&quot; x2=&quot;1204.07&quot; y2=&quot;2398.18&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;2378.06&quot; x2=&quot;1204.07&quot; y2=&quot;2375.09&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;2354.98&quot; x2=&quot;1204.07&quot; y2=&quot;2352.14&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;2331.89&quot; x2=&quot;1204.07&quot; y2=&quot;2329.06&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;2308.81&quot; x2=&quot;1204.07&quot; y2=&quot;2305.97&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;2285.72&quot; x2=&quot;1204.07&quot; y2=&quot;2282.89&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;2262.64&quot; x2=&quot;1204.07&quot; y2=&quot;2259.80&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;2239.68&quot; x2=&quot;1204.07&quot; y2=&quot;2236.71&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;2216.60&quot; x2=&quot;1204.07&quot; y2=&quot;2213.76&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;2193.51&quot; x2=&quot;1204.07&quot; y2=&quot;2190.68&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;2170.43&quot; x2=&quot;1204.07&quot; y2=&quot;2167.59&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;2147.34&quot; x2=&quot;1204.07&quot; y2=&quot;2144.51&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;2124.26&quot; x2=&quot;1204.07&quot; y2=&quot;2121.42&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;2101.31&quot; x2=&quot;1204.07&quot; y2=&quot;2098.34&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;2078.22&quot; x2=&quot;1204.07&quot; y2=&quot;2075.39&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;2055.14&quot; x2=&quot;1204.07&quot; y2=&quot;2052.30&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;2032.05&quot; x2=&quot;1204.07&quot; y2=&quot;2029.22&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;2008.97&quot; x2=&quot;1204.07&quot; y2=&quot;2006.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;1985.88&quot; x2=&quot;1204.07&quot; y2=&quot;1983.05&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;1962.93&quot; x2=&quot;1204.07&quot; y2=&quot;1959.96&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;1939.85&quot; x2=&quot;1204.07&quot; y2=&quot;1937.01&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;1916.76&quot; x2=&quot;1204.07&quot; y2=&quot;1913.92&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;1893.67&quot; x2=&quot;1204.07&quot; y2=&quot;1890.84&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;1870.59&quot; x2=&quot;1204.07&quot; y2=&quot;1867.75&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;1847.50&quot; x2=&quot;1204.07&quot; y2=&quot;1844.67&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;1824.55&quot; x2=&quot;1204.07&quot; y2=&quot;1821.58&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;1801.47&quot; x2=&quot;1204.07&quot; y2=&quot;1798.63&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;1778.38&quot; x2=&quot;1204.07&quot; y2=&quot;1775.55&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;1755.30&quot; x2=&quot;1204.07&quot; y2=&quot;1752.46&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;1732.21&quot; x2=&quot;1204.07&quot; y2=&quot;1729.38&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;1709.26&quot; x2=&quot;1204.07&quot; y2=&quot;1706.29&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;1686.18&quot; x2=&quot;1204.07&quot; y2=&quot;1683.34&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;1663.09&quot; x2=&quot;1204.07&quot; y2=&quot;1660.26&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;1640.01&quot; x2=&quot;1204.07&quot; y2=&quot;1637.17&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;1616.92&quot; x2=&quot;1204.07&quot; y2=&quot;1614.09&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;1593.83&quot; x2=&quot;1204.07&quot; y2=&quot;1591.00&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;1570.88&quot; x2=&quot;1204.07&quot; y2=&quot;1567.91&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;1547.80&quot; x2=&quot;1204.07&quot; y2=&quot;1544.96&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;1524.71&quot; x2=&quot;1204.07&quot; y2=&quot;1521.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;1501.63&quot; x2=&quot;1204.07&quot; y2=&quot;1498.79&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;1478.54&quot; x2=&quot;1204.07&quot; y2=&quot;1475.71&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;1455.46&quot; x2=&quot;1204.07&quot; y2=&quot;1452.62&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;1432.51&quot; x2=&quot;1204.07&quot; y2=&quot;1429.54&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;1409.42&quot; x2=&quot;1204.07&quot; y2=&quot;1406.59&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;1386.34&quot; x2=&quot;1204.07&quot; y2=&quot;1383.50&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;1363.25&quot; x2=&quot;1204.07&quot; y2=&quot;1360.42&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;1340.17&quot; x2=&quot;1204.07&quot; y2=&quot;1337.33&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;1317.08&quot; x2=&quot;1204.07&quot; y2=&quot;1314.25&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;1294.13&quot; x2=&quot;1204.07&quot; y2=&quot;1291.16&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;1271.04&quot; x2=&quot;1204.07&quot; y2=&quot;1268.21&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;1247.96&quot; x2=&quot;1204.07&quot; y2=&quot;1245.12&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;1224.87&quot; x2=&quot;1204.07&quot; y2=&quot;1222.04&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;1201.79&quot; x2=&quot;1204.07&quot; y2=&quot;1198.95&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;1178.70&quot; x2=&quot;1204.07&quot; y2=&quot;1175.87&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;1155.75&quot; x2=&quot;1204.07&quot; y2=&quot;1152.78&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;1132.67&quot; x2=&quot;1204.07&quot; y2=&quot;1129.83&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;1109.58&quot; x2=&quot;1204.07&quot; y2=&quot;1106.75&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;1086.50&quot; x2=&quot;1204.07&quot; y2=&quot;1083.66&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;1063.41&quot; x2=&quot;1204.07&quot; y2=&quot;1060.58&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;1040.33&quot; x2=&quot;1204.07&quot; y2=&quot;1037.49&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;1017.38&quot; x2=&quot;1204.07&quot; y2=&quot;1014.41&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;994.29&quot; x2=&quot;1204.07&quot; y2=&quot;991.46&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;971.21&quot; x2=&quot;1204.07&quot; y2=&quot;968.37&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;948.12&quot; x2=&quot;1204.07&quot; y2=&quot;945.28&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;925.03&quot; x2=&quot;1204.07&quot; y2=&quot;922.20&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;901.95&quot; x2=&quot;1204.07&quot; y2=&quot;899.11&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;879.00&quot; x2=&quot;1204.07&quot; y2=&quot;876.03&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;855.91&quot; x2=&quot;1204.07&quot; y2=&quot;853.08&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;832.83&quot; x2=&quot;1204.07&quot; y2=&quot;829.99&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;809.74&quot; x2=&quot;1204.07&quot; y2=&quot;806.91&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;786.66&quot; x2=&quot;1204.07&quot; y2=&quot;783.82&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;763.57&quot; x2=&quot;1204.07&quot; y2=&quot;760.74&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;740.62&quot; x2=&quot;1204.07&quot; y2=&quot;737.65&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;717.54&quot; x2=&quot;1204.07&quot; y2=&quot;714.70&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;694.45&quot; x2=&quot;1204.07&quot; y2=&quot;691.62&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;671.37&quot; x2=&quot;1204.07&quot; y2=&quot;668.53&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;648.28&quot; x2=&quot;1204.07&quot; y2=&quot;645.45&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;625.19&quot; x2=&quot;1204.07&quot; y2=&quot;622.36&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;602.24&quot; x2=&quot;1204.07&quot; y2=&quot;599.27&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;579.16&quot; x2=&quot;1204.07&quot; y2=&quot;576.32&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;556.07&quot; x2=&quot;1204.07&quot; y2=&quot;553.24&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;532.99&quot; x2=&quot;1204.07&quot; y2=&quot;530.15&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;509.90&quot; x2=&quot;1204.07&quot; y2=&quot;507.07&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;486.82&quot; x2=&quot;1204.07&quot; y2=&quot;483.98&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;463.87&quot; x2=&quot;1204.07&quot; y2=&quot;460.90&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;440.78&quot; x2=&quot;1204.07&quot; y2=&quot;437.95&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;417.70&quot; x2=&quot;1204.07&quot; y2=&quot;414.86&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;394.61&quot; x2=&quot;1204.07&quot; y2=&quot;391.78&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;371.53&quot; x2=&quot;1204.07&quot; y2=&quot;368.69&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;348.44&quot; x2=&quot;1204.07&quot; y2=&quot;345.61&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;325.49&quot; x2=&quot;1204.07&quot; y2=&quot;322.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;302.40&quot; x2=&quot;1204.07&quot; y2=&quot;299.57&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;279.32&quot; x2=&quot;1204.07&quot; y2=&quot;276.48&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;256.23&quot; x2=&quot;1204.07&quot; y2=&quot;253.40&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1506.33&quot; y1=&quot;2608.65&quot; x2=&quot;1506.33&quot; y2=&quot;2605.81&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1506.33&quot; y1=&quot;2585.56&quot; x2=&quot;1506.33&quot; y2=&quot;2582.73&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1506.33&quot; y1=&quot;2562.48&quot; x2=&quot;1506.33&quot; y2=&quot;2559.64&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1506.33&quot; y1=&quot;2539.39&quot; x2=&quot;1506.33&quot; y2=&quot;2536.55&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1506.33&quot; y1=&quot;2516.44&quot; x2=&quot;1506.33&quot; y2=&quot;2513.47&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1506.33&quot; y1=&quot;2493.35&quot; x2=&quot;1506.33&quot; y2=&quot;2490.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1506.33&quot; y1=&quot;2470.27&quot; x2=&quot;1506.33&quot; y2=&quot;2467.43&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1506.33&quot; y1=&quot;2447.18&quot; x2=&quot;1506.33&quot; y2=&quot;2444.35&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1506.33&quot; y1=&quot;2424.10&quot; x2=&quot;1506.33&quot; y2=&quot;2421.26&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1506.33&quot; y1=&quot;2401.01&quot; x2=&quot;1506.33&quot; y2=&quot;2398.18&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1506.33&quot; y1=&quot;2378.06&quot; x2=&quot;1506.33&quot; y2=&quot;2375.09&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1506.33&quot; y1=&quot;2354.98&quot; x2=&quot;1506.33&quot; y2=&quot;2352.14&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1506.33&quot; y1=&quot;2331.89&quot; x2=&quot;1506.33&quot; y2=&quot;2329.06&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1506.33&quot; y1=&quot;2308.81&quot; x2=&quot;1506.33&quot; y2=&quot;2305.97&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1506.33&quot; y1=&quot;2285.72&quot; x2=&quot;1506.33&quot; y2=&quot;2282.89&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1506.33&quot; y1=&quot;2262.64&quot; x2=&quot;1506.33&quot; y2=&quot;2259.80&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1506.33&quot; y1=&quot;2239.68&quot; x2=&quot;1506.33&quot; y2=&quot;2236.71&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1506.33&quot; y1=&quot;2216.60&quot; x2=&quot;1506.33&quot; y2=&quot;2213.76&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1506.33&quot; y1=&quot;2193.51&quot; x2=&quot;1506.33&quot; y2=&quot;2190.68&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1506.33&quot; y1=&quot;2170.43&quot; x2=&quot;1506.33&quot; y2=&quot;2167.59&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1506.33&quot; y1=&quot;2147.34&quot; x2=&quot;1506.33&quot; y2=&quot;2144.51&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1506.33&quot; y1=&quot;2124.26&quot; x2=&quot;1506.33&quot; y2=&quot;2121.42&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1506.33&quot; y1=&quot;2101.31&quot; x2=&quot;1506.33&quot; y2=&quot;2098.34&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1506.33&quot; y1=&quot;2078.22&quot; x2=&quot;1506.33&quot; y2=&quot;2075.39&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1506.33&quot; y1=&quot;2055.14&quot; x2=&quot;1506.33&quot; y2=&quot;2052.30&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1506.33&quot; y1=&quot;2032.05&quot; x2=&quot;1506.33&quot; y2=&quot;2029.22&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1506.33&quot; y1=&quot;2008.97&quot; x2=&quot;1506.33&quot; y2=&quot;2006.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1506.33&quot; y1=&quot;1985.88&quot; x2=&quot;1506.33&quot; y2=&quot;1983.05&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1506.33&quot; y1=&quot;1962.93&quot; x2=&quot;1506.33&quot; y2=&quot;1959.96&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1506.33&quot; y1=&quot;1939.85&quot; x2=&quot;1506.33&quot; y2=&quot;1937.01&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1506.33&quot; y1=&quot;1916.76&quot; x2=&quot;1506.33&quot; y2=&quot;1913.92&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1506.33&quot; y1=&quot;1893.67&quot; x2=&quot;1506.33&quot; y2=&quot;1890.84&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1506.33&quot; y1=&quot;1870.59&quot; x2=&quot;1506.33&quot; y2=&quot;1867.75&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1506.33&quot; y1=&quot;1847.50&quot; x2=&quot;1506.33&quot; y2=&quot;1844.67&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1506.33&quot; y1=&quot;1824.55&quot; x2=&quot;1506.33&quot; y2=&quot;1821.58&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1506.33&quot; y1=&quot;1801.47&quot; x2=&quot;1506.33&quot; y2=&quot;1798.63&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1506.33&quot; y1=&quot;1778.38&quot; x2=&quot;1506.33&quot; y2=&quot;1775.55&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1506.33&quot; y1=&quot;1755.30&quot; x2=&quot;1506.33&quot; y2=&quot;1752.46&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1506.33&quot; y1=&quot;1732.21&quot; x2=&quot;1506.33&quot; y2=&quot;1729.38&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1506.33&quot; y1=&quot;1709.26&quot; x2=&quot;1506.33&quot; y2=&quot;1706.29&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1506.33&quot; y1=&quot;1686.18&quot; x2=&quot;1506.33&quot; y2=&quot;1683.34&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1506.33&quot; y1=&quot;1663.09&quot; x2=&quot;1506.33&quot; y2=&quot;1660.26&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1506.33&quot; y1=&quot;1640.01&quot; x2=&quot;1506.33&quot; y2=&quot;1637.17&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1506.33&quot; y1=&quot;1616.92&quot; x2=&quot;1506.33&quot; y2=&quot;1614.09&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1506.33&quot; y1=&quot;1593.83&quot; x2=&quot;1506.33&quot; y2=&quot;1591.00&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1506.33&quot; y1=&quot;1570.88&quot; x2=&quot;1506.33&quot; y2=&quot;1567.91&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1506.33&quot; y1=&quot;1547.80&quot; x2=&quot;1506.33&quot; y2=&quot;1544.96&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1506.33&quot; y1=&quot;1524.71&quot; x2=&quot;1506.33&quot; y2=&quot;1521.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1506.33&quot; y1=&quot;1501.63&quot; x2=&quot;1506.33&quot; y2=&quot;1498.79&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1506.33&quot; y1=&quot;1478.54&quot; x2=&quot;1506.33&quot; y2=&quot;1475.71&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1506.33&quot; y1=&quot;1455.46&quot; x2=&quot;1506.33&quot; y2=&quot;1452.62&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1506.33&quot; y1=&quot;1432.51&quot; x2=&quot;1506.33&quot; y2=&quot;1429.54&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1506.33&quot; y1=&quot;1409.42&quot; x2=&quot;1506.33&quot; y2=&quot;1406.59&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1506.33&quot; y1=&quot;1386.34&quot; x2=&quot;1506.33&quot; y2=&quot;1383.50&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1506.33&quot; y1=&quot;1363.25&quot; x2=&quot;1506.33&quot; y2=&quot;1360.42&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1506.33&quot; y1=&quot;1340.17&quot; x2=&quot;1506.33&quot; y2=&quot;1337.33&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1506.33&quot; y1=&quot;1317.08&quot; x2=&quot;1506.33&quot; y2=&quot;1314.25&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1506.33&quot; y1=&quot;1294.13&quot; x2=&quot;1506.33&quot; y2=&quot;1291.16&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1506.33&quot; y1=&quot;1271.04&quot; x2=&quot;1506.33&quot; y2=&quot;1268.21&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1506.33&quot; y1=&quot;1247.96&quot; x2=&quot;1506.33&quot; y2=&quot;1245.12&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1506.33&quot; y1=&quot;1224.87&quot; x2=&quot;1506.33&quot; y2=&quot;1222.04&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1506.33&quot; y1=&quot;1201.79&quot; x2=&quot;1506.33&quot; y2=&quot;1198.95&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1506.33&quot; y1=&quot;1178.70&quot; x2=&quot;1506.33&quot; y2=&quot;1175.87&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1506.33&quot; y1=&quot;1155.75&quot; x2=&quot;1506.33&quot; y2=&quot;1152.78&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1506.33&quot; y1=&quot;1132.67&quot; x2=&quot;1506.33&quot; y2=&quot;1129.83&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1506.33&quot; y1=&quot;1109.58&quot; x2=&quot;1506.33&quot; y2=&quot;1106.75&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1506.33&quot; y1=&quot;1086.50&quot; x2=&quot;1506.33&quot; y2=&quot;1083.66&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1506.33&quot; y1=&quot;1063.41&quot; x2=&quot;1506.33&quot; y2=&quot;1060.58&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1506.33&quot; y1=&quot;1040.33&quot; x2=&quot;1506.33&quot; y2=&quot;1037.49&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1506.33&quot; y1=&quot;1017.38&quot; x2=&quot;1506.33&quot; y2=&quot;1014.41&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1506.33&quot; y1=&quot;994.29&quot; x2=&quot;1506.33&quot; y2=&quot;991.46&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1506.33&quot; y1=&quot;971.21&quot; x2=&quot;1506.33&quot; y2=&quot;968.37&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1506.33&quot; y1=&quot;948.12&quot; x2=&quot;1506.33&quot; y2=&quot;945.28&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1506.33&quot; y1=&quot;925.03&quot; x2=&quot;1506.33&quot; y2=&quot;922.20&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1506.33&quot; y1=&quot;901.95&quot; x2=&quot;1506.33&quot; y2=&quot;899.11&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1506.33&quot; y1=&quot;879.00&quot; x2=&quot;1506.33&quot; y2=&quot;876.03&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1506.33&quot; y1=&quot;855.91&quot; x2=&quot;1506.33&quot; y2=&quot;853.08&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1506.33&quot; y1=&quot;832.83&quot; x2=&quot;1506.33&quot; y2=&quot;829.99&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1506.33&quot; y1=&quot;809.74&quot; x2=&quot;1506.33&quot; y2=&quot;806.91&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1506.33&quot; y1=&quot;786.66&quot; x2=&quot;1506.33&quot; y2=&quot;783.82&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1506.33&quot; y1=&quot;763.57&quot; x2=&quot;1506.33&quot; y2=&quot;760.74&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1506.33&quot; y1=&quot;740.62&quot; x2=&quot;1506.33&quot; y2=&quot;737.65&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1506.33&quot; y1=&quot;717.54&quot; x2=&quot;1506.33&quot; y2=&quot;714.70&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1506.33&quot; y1=&quot;694.45&quot; x2=&quot;1506.33&quot; y2=&quot;691.62&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1506.33&quot; y1=&quot;671.37&quot; x2=&quot;1506.33&quot; y2=&quot;668.53&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1506.33&quot; y1=&quot;648.28&quot; x2=&quot;1506.33&quot; y2=&quot;645.45&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1506.33&quot; y1=&quot;625.19&quot; x2=&quot;1506.33&quot; y2=&quot;622.36&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1506.33&quot; y1=&quot;602.24&quot; x2=&quot;1506.33&quot; y2=&quot;599.27&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1506.33&quot; y1=&quot;579.16&quot; x2=&quot;1506.33&quot; y2=&quot;576.32&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1506.33&quot; y1=&quot;556.07&quot; x2=&quot;1506.33&quot; y2=&quot;553.24&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1506.33&quot; y1=&quot;532.99&quot; x2=&quot;1506.33&quot; y2=&quot;530.15&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1506.33&quot; y1=&quot;509.90&quot; x2=&quot;1506.33&quot; y2=&quot;507.07&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1506.33&quot; y1=&quot;486.82&quot; x2=&quot;1506.33&quot; y2=&quot;483.98&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1506.33&quot; y1=&quot;463.87&quot; x2=&quot;1506.33&quot; y2=&quot;460.90&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1506.33&quot; y1=&quot;440.78&quot; x2=&quot;1506.33&quot; y2=&quot;437.95&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1506.33&quot; y1=&quot;417.70&quot; x2=&quot;1506.33&quot; y2=&quot;414.86&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1506.33&quot; y1=&quot;394.61&quot; x2=&quot;1506.33&quot; y2=&quot;391.78&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1506.33&quot; y1=&quot;371.53&quot; x2=&quot;1506.33&quot; y2=&quot;368.69&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1506.33&quot; y1=&quot;348.44&quot; x2=&quot;1506.33&quot; y2=&quot;345.61&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1506.33&quot; y1=&quot;325.49&quot; x2=&quot;1506.33&quot; y2=&quot;322.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1506.33&quot; y1=&quot;302.40&quot; x2=&quot;1506.33&quot; y2=&quot;299.57&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1506.33&quot; y1=&quot;279.32&quot; x2=&quot;1506.33&quot; y2=&quot;276.48&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1506.33&quot; y1=&quot;256.23&quot; x2=&quot;1506.33&quot; y2=&quot;253.40&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;2608.65&quot; x2=&quot;1204.07&quot; y2=&quot;2585.56&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;2565.45&quot; x2=&quot;1204.07&quot; y2=&quot;2542.36&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;2522.24&quot; x2=&quot;1204.07&quot; y2=&quot;2499.16&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;2479.04&quot; x2=&quot;1204.07&quot; y2=&quot;2455.96&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;2435.84&quot; x2=&quot;1204.07&quot; y2=&quot;2412.76&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;2392.64&quot; x2=&quot;1204.07&quot; y2=&quot;2369.56&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;2349.44&quot; x2=&quot;1204.07&quot; y2=&quot;2326.36&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;2306.24&quot; x2=&quot;1204.07&quot; y2=&quot;2283.16&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;2263.04&quot; x2=&quot;1204.07&quot; y2=&quot;2239.95&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;2219.84&quot; x2=&quot;1204.07&quot; y2=&quot;2196.75&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;2176.64&quot; x2=&quot;1204.07&quot; y2=&quot;2153.55&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;2133.44&quot; x2=&quot;1204.07&quot; y2=&quot;2110.35&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;2090.24&quot; x2=&quot;1204.07&quot; y2=&quot;2067.15&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;2047.04&quot; x2=&quot;1204.07&quot; y2=&quot;2023.95&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;2003.84&quot; x2=&quot;1204.07&quot; y2=&quot;1980.75&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;1960.64&quot; x2=&quot;1204.07&quot; y2=&quot;1937.55&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;1917.43&quot; x2=&quot;1204.07&quot; y2=&quot;1894.35&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;1874.23&quot; x2=&quot;1204.07&quot; y2=&quot;1851.15&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;1831.03&quot; x2=&quot;1204.07&quot; y2=&quot;1807.95&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;1787.83&quot; x2=&quot;1204.07&quot; y2=&quot;1764.75&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;1744.63&quot; x2=&quot;1204.07&quot; y2=&quot;1721.55&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;1701.43&quot; x2=&quot;1204.07&quot; y2=&quot;1678.35&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;1658.23&quot; x2=&quot;1204.07&quot; y2=&quot;1635.15&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;1615.03&quot; x2=&quot;1204.07&quot; y2=&quot;1591.94&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;1571.83&quot; x2=&quot;1204.07&quot; y2=&quot;1548.74&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;1528.63&quot; x2=&quot;1204.07&quot; y2=&quot;1505.54&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;1485.43&quot; x2=&quot;1204.07&quot; y2=&quot;1462.34&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;1442.23&quot; x2=&quot;1204.07&quot; y2=&quot;1419.14&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;1399.03&quot; x2=&quot;1204.07&quot; y2=&quot;1375.94&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;1355.83&quot; x2=&quot;1204.07&quot; y2=&quot;1332.74&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;1312.63&quot; x2=&quot;1204.07&quot; y2=&quot;1289.54&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;1269.42&quot; x2=&quot;1204.07&quot; y2=&quot;1246.34&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;1226.22&quot; x2=&quot;1204.07&quot; y2=&quot;1203.14&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;1183.02&quot; x2=&quot;1204.07&quot; y2=&quot;1159.94&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;1139.82&quot; x2=&quot;1204.07&quot; y2=&quot;1116.74&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;1096.62&quot; x2=&quot;1204.07&quot; y2=&quot;1073.54&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;1053.42&quot; x2=&quot;1204.07&quot; y2=&quot;1030.34&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;1010.22&quot; x2=&quot;1204.07&quot; y2=&quot;987.14&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;967.02&quot; x2=&quot;1204.07&quot; y2=&quot;943.93&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;923.82&quot; x2=&quot;1204.07&quot; y2=&quot;900.73&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;880.62&quot; x2=&quot;1204.07&quot; y2=&quot;857.53&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;837.42&quot; x2=&quot;1204.07&quot; y2=&quot;814.33&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;794.22&quot; x2=&quot;1204.07&quot; y2=&quot;771.13&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;751.02&quot; x2=&quot;1204.07&quot; y2=&quot;727.93&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;707.82&quot; x2=&quot;1204.07&quot; y2=&quot;684.73&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;664.62&quot; x2=&quot;1204.07&quot; y2=&quot;641.53&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;621.41&quot; x2=&quot;1204.07&quot; y2=&quot;598.33&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;578.21&quot; x2=&quot;1204.07&quot; y2=&quot;555.13&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;535.01&quot; x2=&quot;1204.07&quot; y2=&quot;511.93&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;491.81&quot; x2=&quot;1204.07&quot; y2=&quot;468.73&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;448.61&quot; x2=&quot;1204.07&quot; y2=&quot;425.53&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;405.41&quot; x2=&quot;1204.07&quot; y2=&quot;382.33&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;362.21&quot; x2=&quot;1204.07&quot; y2=&quot;339.13&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;319.01&quot; x2=&quot;1204.07&quot; y2=&quot;295.92&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;275.81&quot; x2=&quot;1204.07&quot; y2=&quot;252.72&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;1931.44&quot; y1=&quot;642.88&quot; x2=&quot;1934.28&quot; y2=&quot;642.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1954.53&quot; y1=&quot;642.88&quot; x2=&quot;1957.37&quot; y2=&quot;642.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1977.62&quot; y1=&quot;642.88&quot; x2=&quot;1980.45&quot; y2=&quot;642.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2000.57&quot; y1=&quot;642.88&quot; x2=&quot;2003.54&quot; y2=&quot;642.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2023.65&quot; y1=&quot;642.88&quot; x2=&quot;2026.48&quot; y2=&quot;642.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2046.73&quot; y1=&quot;642.88&quot; x2=&quot;2049.57&quot; y2=&quot;642.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2069.82&quot; y1=&quot;642.88&quot; x2=&quot;2072.66&quot; y2=&quot;642.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2092.91&quot; y1=&quot;642.88&quot; x2=&quot;2095.74&quot; y2=&quot;642.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2115.99&quot; y1=&quot;642.88&quot; x2=&quot;2118.82&quot; y2=&quot;642.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2138.94&quot; y1=&quot;642.88&quot; x2=&quot;2141.91&quot; y2=&quot;642.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2162.03&quot; y1=&quot;642.88&quot; x2=&quot;2164.86&quot; y2=&quot;642.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2185.11&quot; y1=&quot;642.88&quot; x2=&quot;2187.95&quot; y2=&quot;642.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2208.20&quot; y1=&quot;642.88&quot; x2=&quot;2211.03&quot; y2=&quot;642.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2231.28&quot; y1=&quot;642.88&quot; x2=&quot;2234.11&quot; y2=&quot;642.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2254.36&quot; y1=&quot;642.88&quot; x2=&quot;2257.20&quot; y2=&quot;642.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2277.32&quot; y1=&quot;642.88&quot; x2=&quot;2280.28&quot; y2=&quot;642.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2300.40&quot; y1=&quot;642.88&quot; x2=&quot;2303.24&quot; y2=&quot;642.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2323.49&quot; y1=&quot;642.88&quot; x2=&quot;2326.32&quot; y2=&quot;642.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2346.57&quot; y1=&quot;642.88&quot; x2=&quot;2349.41&quot; y2=&quot;642.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2369.66&quot; y1=&quot;642.88&quot; x2=&quot;2372.49&quot; y2=&quot;642.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2392.74&quot; y1=&quot;642.88&quot; x2=&quot;2395.57&quot; y2=&quot;642.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2415.69&quot; y1=&quot;642.88&quot; x2=&quot;2418.66&quot; y2=&quot;642.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2438.78&quot; y1=&quot;642.88&quot; x2=&quot;2441.61&quot; y2=&quot;642.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2461.86&quot; y1=&quot;642.88&quot; x2=&quot;2464.70&quot; y2=&quot;642.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2484.95&quot; y1=&quot;642.88&quot; x2=&quot;2487.78&quot; y2=&quot;642.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2508.03&quot; y1=&quot;642.88&quot; x2=&quot;2510.86&quot; y2=&quot;642.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2531.11&quot; y1=&quot;642.88&quot; x2=&quot;2533.95&quot; y2=&quot;642.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2554.07&quot; y1=&quot;642.88&quot; x2=&quot;2557.03&quot; y2=&quot;642.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2577.15&quot; y1=&quot;642.88&quot; x2=&quot;2579.99&quot; y2=&quot;642.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2600.24&quot; y1=&quot;642.88&quot; x2=&quot;2603.07&quot; y2=&quot;642.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2623.32&quot; y1=&quot;642.88&quot; x2=&quot;2626.16&quot; y2=&quot;642.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2646.41&quot; y1=&quot;642.88&quot; x2=&quot;2649.24&quot; y2=&quot;642.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2669.49&quot; y1=&quot;642.88&quot; x2=&quot;2672.32&quot; y2=&quot;642.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2692.44&quot; y1=&quot;642.88&quot; x2=&quot;2695.41&quot; y2=&quot;642.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2715.53&quot; y1=&quot;642.88&quot; x2=&quot;2718.36&quot; y2=&quot;642.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2738.61&quot; y1=&quot;642.88&quot; x2=&quot;2741.45&quot; y2=&quot;642.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2761.70&quot; y1=&quot;642.88&quot; x2=&quot;2764.53&quot; y2=&quot;642.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2784.78&quot; y1=&quot;642.88&quot; x2=&quot;2787.61&quot; y2=&quot;642.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2807.86&quot; y1=&quot;642.88&quot; x2=&quot;2810.70&quot; y2=&quot;642.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2830.82&quot; y1=&quot;642.88&quot; x2=&quot;2833.78&quot; y2=&quot;642.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2853.90&quot; y1=&quot;642.88&quot; x2=&quot;2856.74&quot; y2=&quot;642.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2876.99&quot; y1=&quot;642.88&quot; x2=&quot;2879.82&quot; y2=&quot;642.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2900.07&quot; y1=&quot;642.88&quot; x2=&quot;2902.91&quot; y2=&quot;642.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2923.16&quot; y1=&quot;642.88&quot; x2=&quot;2925.99&quot; y2=&quot;642.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2946.24&quot; y1=&quot;642.88&quot; x2=&quot;2949.07&quot; y2=&quot;642.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2969.19&quot; y1=&quot;642.88&quot; x2=&quot;2972.16&quot; y2=&quot;642.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2992.28&quot; y1=&quot;642.88&quot; x2=&quot;2995.11&quot; y2=&quot;642.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3015.36&quot; y1=&quot;642.88&quot; x2=&quot;3018.20&quot; y2=&quot;642.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1931.44&quot; y1=&quot;1429.13&quot; x2=&quot;1934.28&quot; y2=&quot;1429.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1954.53&quot; y1=&quot;1429.13&quot; x2=&quot;1957.37&quot; y2=&quot;1429.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1977.62&quot; y1=&quot;1429.13&quot; x2=&quot;1980.45&quot; y2=&quot;1429.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2000.57&quot; y1=&quot;1429.13&quot; x2=&quot;2003.54&quot; y2=&quot;1429.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2023.65&quot; y1=&quot;1429.13&quot; x2=&quot;2026.48&quot; y2=&quot;1429.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2046.73&quot; y1=&quot;1429.13&quot; x2=&quot;2049.57&quot; y2=&quot;1429.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2069.82&quot; y1=&quot;1429.13&quot; x2=&quot;2072.66&quot; y2=&quot;1429.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2092.91&quot; y1=&quot;1429.13&quot; x2=&quot;2095.74&quot; y2=&quot;1429.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2115.99&quot; y1=&quot;1429.13&quot; x2=&quot;2118.82&quot; y2=&quot;1429.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2138.94&quot; y1=&quot;1429.13&quot; x2=&quot;2141.91&quot; y2=&quot;1429.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2162.03&quot; y1=&quot;1429.13&quot; x2=&quot;2164.86&quot; y2=&quot;1429.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2185.11&quot; y1=&quot;1429.13&quot; x2=&quot;2187.95&quot; y2=&quot;1429.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2208.20&quot; y1=&quot;1429.13&quot; x2=&quot;2211.03&quot; y2=&quot;1429.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2231.28&quot; y1=&quot;1429.13&quot; x2=&quot;2234.11&quot; y2=&quot;1429.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2254.36&quot; y1=&quot;1429.13&quot; x2=&quot;2257.20&quot; y2=&quot;1429.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2277.32&quot; y1=&quot;1429.13&quot; x2=&quot;2280.28&quot; y2=&quot;1429.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2300.40&quot; y1=&quot;1429.13&quot; x2=&quot;2303.24&quot; y2=&quot;1429.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2323.49&quot; y1=&quot;1429.13&quot; x2=&quot;2326.32&quot; y2=&quot;1429.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2346.57&quot; y1=&quot;1429.13&quot; x2=&quot;2349.41&quot; y2=&quot;1429.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2369.66&quot; y1=&quot;1429.13&quot; x2=&quot;2372.49&quot; y2=&quot;1429.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2392.74&quot; y1=&quot;1429.13&quot; x2=&quot;2395.57&quot; y2=&quot;1429.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2415.69&quot; y1=&quot;1429.13&quot; x2=&quot;2418.66&quot; y2=&quot;1429.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2438.78&quot; y1=&quot;1429.13&quot; x2=&quot;2441.61&quot; y2=&quot;1429.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2461.86&quot; y1=&quot;1429.13&quot; x2=&quot;2464.70&quot; y2=&quot;1429.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2484.95&quot; y1=&quot;1429.13&quot; x2=&quot;2487.78&quot; y2=&quot;1429.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2508.03&quot; y1=&quot;1429.13&quot; x2=&quot;2510.86&quot; y2=&quot;1429.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2531.11&quot; y1=&quot;1429.13&quot; x2=&quot;2533.95&quot; y2=&quot;1429.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2554.07&quot; y1=&quot;1429.13&quot; x2=&quot;2557.03&quot; y2=&quot;1429.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2577.15&quot; y1=&quot;1429.13&quot; x2=&quot;2579.99&quot; y2=&quot;1429.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2600.24&quot; y1=&quot;1429.13&quot; x2=&quot;2603.07&quot; y2=&quot;1429.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2623.32&quot; y1=&quot;1429.13&quot; x2=&quot;2626.16&quot; y2=&quot;1429.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2646.41&quot; y1=&quot;1429.13&quot; x2=&quot;2649.24&quot; y2=&quot;1429.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2669.49&quot; y1=&quot;1429.13&quot; x2=&quot;2672.32&quot; y2=&quot;1429.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2692.44&quot; y1=&quot;1429.13&quot; x2=&quot;2695.41&quot; y2=&quot;1429.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2715.53&quot; y1=&quot;1429.13&quot; x2=&quot;2718.36&quot; y2=&quot;1429.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2738.61&quot; y1=&quot;1429.13&quot; x2=&quot;2741.45&quot; y2=&quot;1429.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2761.70&quot; y1=&quot;1429.13&quot; x2=&quot;2764.53&quot; y2=&quot;1429.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2784.78&quot; y1=&quot;1429.13&quot; x2=&quot;2787.61&quot; y2=&quot;1429.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2807.86&quot; y1=&quot;1429.13&quot; x2=&quot;2810.70&quot; y2=&quot;1429.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2830.82&quot; y1=&quot;1429.13&quot; x2=&quot;2833.78&quot; y2=&quot;1429.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2853.90&quot; y1=&quot;1429.13&quot; x2=&quot;2856.74&quot; y2=&quot;1429.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2876.99&quot; y1=&quot;1429.13&quot; x2=&quot;2879.82&quot; y2=&quot;1429.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2900.07&quot; y1=&quot;1429.13&quot; x2=&quot;2902.91&quot; y2=&quot;1429.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2923.16&quot; y1=&quot;1429.13&quot; x2=&quot;2925.99&quot; y2=&quot;1429.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2946.24&quot; y1=&quot;1429.13&quot; x2=&quot;2949.07&quot; y2=&quot;1429.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2969.19&quot; y1=&quot;1429.13&quot; x2=&quot;2972.16&quot; y2=&quot;1429.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2992.28&quot; y1=&quot;1429.13&quot; x2=&quot;2995.11&quot; y2=&quot;1429.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3015.36&quot; y1=&quot;1429.13&quot; x2=&quot;3018.20&quot; y2=&quot;1429.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1931.44&quot; y1=&quot;2215.52&quot; x2=&quot;1934.28&quot; y2=&quot;2215.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1954.53&quot; y1=&quot;2215.52&quot; x2=&quot;1957.37&quot; y2=&quot;2215.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;1977.62&quot; y1=&quot;2215.52&quot; x2=&quot;1980.45&quot; y2=&quot;2215.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2000.57&quot; y1=&quot;2215.52&quot; x2=&quot;2003.54&quot; y2=&quot;2215.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2023.65&quot; y1=&quot;2215.52&quot; x2=&quot;2026.48&quot; y2=&quot;2215.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2046.73&quot; y1=&quot;2215.52&quot; x2=&quot;2049.57&quot; y2=&quot;2215.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2069.82&quot; y1=&quot;2215.52&quot; x2=&quot;2072.66&quot; y2=&quot;2215.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2092.91&quot; y1=&quot;2215.52&quot; x2=&quot;2095.74&quot; y2=&quot;2215.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2115.99&quot; y1=&quot;2215.52&quot; x2=&quot;2118.82&quot; y2=&quot;2215.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2138.94&quot; y1=&quot;2215.52&quot; x2=&quot;2141.91&quot; y2=&quot;2215.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2162.03&quot; y1=&quot;2215.52&quot; x2=&quot;2164.86&quot; y2=&quot;2215.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2185.11&quot; y1=&quot;2215.52&quot; x2=&quot;2187.95&quot; y2=&quot;2215.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2208.20&quot; y1=&quot;2215.52&quot; x2=&quot;2211.03&quot; y2=&quot;2215.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2231.28&quot; y1=&quot;2215.52&quot; x2=&quot;2234.11&quot; y2=&quot;2215.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2254.36&quot; y1=&quot;2215.52&quot; x2=&quot;2257.20&quot; y2=&quot;2215.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2277.32&quot; y1=&quot;2215.52&quot; x2=&quot;2280.28&quot; y2=&quot;2215.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2300.40&quot; y1=&quot;2215.52&quot; x2=&quot;2303.24&quot; y2=&quot;2215.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2323.49&quot; y1=&quot;2215.52&quot; x2=&quot;2326.32&quot; y2=&quot;2215.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2346.57&quot; y1=&quot;2215.52&quot; x2=&quot;2349.41&quot; y2=&quot;2215.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2369.66&quot; y1=&quot;2215.52&quot; x2=&quot;2372.49&quot; y2=&quot;2215.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2392.74&quot; y1=&quot;2215.52&quot; x2=&quot;2395.57&quot; y2=&quot;2215.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2415.69&quot; y1=&quot;2215.52&quot; x2=&quot;2418.66&quot; y2=&quot;2215.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2438.78&quot; y1=&quot;2215.52&quot; x2=&quot;2441.61&quot; y2=&quot;2215.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2461.86&quot; y1=&quot;2215.52&quot; x2=&quot;2464.70&quot; y2=&quot;2215.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2484.95&quot; y1=&quot;2215.52&quot; x2=&quot;2487.78&quot; y2=&quot;2215.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2508.03&quot; y1=&quot;2215.52&quot; x2=&quot;2510.86&quot; y2=&quot;2215.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2531.11&quot; y1=&quot;2215.52&quot; x2=&quot;2533.95&quot; y2=&quot;2215.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2554.07&quot; y1=&quot;2215.52&quot; x2=&quot;2557.03&quot; y2=&quot;2215.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2577.15&quot; y1=&quot;2215.52&quot; x2=&quot;2579.99&quot; y2=&quot;2215.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2600.24&quot; y1=&quot;2215.52&quot; x2=&quot;2603.07&quot; y2=&quot;2215.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2623.32&quot; y1=&quot;2215.52&quot; x2=&quot;2626.16&quot; y2=&quot;2215.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2646.41&quot; y1=&quot;2215.52&quot; x2=&quot;2649.24&quot; y2=&quot;2215.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2669.49&quot; y1=&quot;2215.52&quot; x2=&quot;2672.32&quot; y2=&quot;2215.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2692.44&quot; y1=&quot;2215.52&quot; x2=&quot;2695.41&quot; y2=&quot;2215.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2715.53&quot; y1=&quot;2215.52&quot; x2=&quot;2718.36&quot; y2=&quot;2215.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2738.61&quot; y1=&quot;2215.52&quot; x2=&quot;2741.45&quot; y2=&quot;2215.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2761.70&quot; y1=&quot;2215.52&quot; x2=&quot;2764.53&quot; y2=&quot;2215.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2784.78&quot; y1=&quot;2215.52&quot; x2=&quot;2787.61&quot; y2=&quot;2215.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2807.86&quot; y1=&quot;2215.52&quot; x2=&quot;2810.70&quot; y2=&quot;2215.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2830.82&quot; y1=&quot;2215.52&quot; x2=&quot;2833.78&quot; y2=&quot;2215.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2853.90&quot; y1=&quot;2215.52&quot; x2=&quot;2856.74&quot; y2=&quot;2215.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2876.99&quot; y1=&quot;2215.52&quot; x2=&quot;2879.82&quot; y2=&quot;2215.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2900.07&quot; y1=&quot;2215.52&quot; x2=&quot;2902.91&quot; y2=&quot;2215.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2923.16&quot; y1=&quot;2215.52&quot; x2=&quot;2925.99&quot; y2=&quot;2215.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2946.24&quot; y1=&quot;2215.52&quot; x2=&quot;2949.07&quot; y2=&quot;2215.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2969.19&quot; y1=&quot;2215.52&quot; x2=&quot;2972.16&quot; y2=&quot;2215.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2992.28&quot; y1=&quot;2215.52&quot; x2=&quot;2995.11&quot; y2=&quot;2215.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3015.36&quot; y1=&quot;2215.52&quot; x2=&quot;3018.20&quot; y2=&quot;2215.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2008.13&quot; y1=&quot;2608.65&quot; x2=&quot;2008.13&quot; y2=&quot;2605.81&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2008.13&quot; y1=&quot;2585.56&quot; x2=&quot;2008.13&quot; y2=&quot;2582.73&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2008.13&quot; y1=&quot;2562.48&quot; x2=&quot;2008.13&quot; y2=&quot;2559.64&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2008.13&quot; y1=&quot;2539.39&quot; x2=&quot;2008.13&quot; y2=&quot;2536.55&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2008.13&quot; y1=&quot;2516.44&quot; x2=&quot;2008.13&quot; y2=&quot;2513.47&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2008.13&quot; y1=&quot;2493.35&quot; x2=&quot;2008.13&quot; y2=&quot;2490.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2008.13&quot; y1=&quot;2470.27&quot; x2=&quot;2008.13&quot; y2=&quot;2467.43&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2008.13&quot; y1=&quot;2447.18&quot; x2=&quot;2008.13&quot; y2=&quot;2444.35&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2008.13&quot; y1=&quot;2424.10&quot; x2=&quot;2008.13&quot; y2=&quot;2421.26&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2008.13&quot; y1=&quot;2401.01&quot; x2=&quot;2008.13&quot; y2=&quot;2398.18&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2008.13&quot; y1=&quot;2378.06&quot; x2=&quot;2008.13&quot; y2=&quot;2375.09&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2008.13&quot; y1=&quot;2354.98&quot; x2=&quot;2008.13&quot; y2=&quot;2352.14&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2008.13&quot; y1=&quot;2331.89&quot; x2=&quot;2008.13&quot; y2=&quot;2329.06&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2008.13&quot; y1=&quot;2308.81&quot; x2=&quot;2008.13&quot; y2=&quot;2305.97&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2008.13&quot; y1=&quot;2285.72&quot; x2=&quot;2008.13&quot; y2=&quot;2282.89&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2008.13&quot; y1=&quot;2262.64&quot; x2=&quot;2008.13&quot; y2=&quot;2259.80&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2008.13&quot; y1=&quot;2239.68&quot; x2=&quot;2008.13&quot; y2=&quot;2236.71&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2008.13&quot; y1=&quot;2216.60&quot; x2=&quot;2008.13&quot; y2=&quot;2213.76&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2008.13&quot; y1=&quot;2193.51&quot; x2=&quot;2008.13&quot; y2=&quot;2190.68&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2008.13&quot; y1=&quot;2170.43&quot; x2=&quot;2008.13&quot; y2=&quot;2167.59&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2008.13&quot; y1=&quot;2147.34&quot; x2=&quot;2008.13&quot; y2=&quot;2144.51&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2008.13&quot; y1=&quot;2124.26&quot; x2=&quot;2008.13&quot; y2=&quot;2121.42&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2008.13&quot; y1=&quot;2101.31&quot; x2=&quot;2008.13&quot; y2=&quot;2098.34&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2008.13&quot; y1=&quot;2078.22&quot; x2=&quot;2008.13&quot; y2=&quot;2075.39&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2008.13&quot; y1=&quot;2055.14&quot; x2=&quot;2008.13&quot; y2=&quot;2052.30&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2008.13&quot; y1=&quot;2032.05&quot; x2=&quot;2008.13&quot; y2=&quot;2029.22&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2008.13&quot; y1=&quot;2008.97&quot; x2=&quot;2008.13&quot; y2=&quot;2006.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2008.13&quot; y1=&quot;1985.88&quot; x2=&quot;2008.13&quot; y2=&quot;1983.05&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2008.13&quot; y1=&quot;1962.93&quot; x2=&quot;2008.13&quot; y2=&quot;1959.96&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2008.13&quot; y1=&quot;1939.85&quot; x2=&quot;2008.13&quot; y2=&quot;1937.01&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2008.13&quot; y1=&quot;1916.76&quot; x2=&quot;2008.13&quot; y2=&quot;1913.92&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2008.13&quot; y1=&quot;1893.67&quot; x2=&quot;2008.13&quot; y2=&quot;1890.84&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2008.13&quot; y1=&quot;1870.59&quot; x2=&quot;2008.13&quot; y2=&quot;1867.75&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2008.13&quot; y1=&quot;1847.50&quot; x2=&quot;2008.13&quot; y2=&quot;1844.67&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2008.13&quot; y1=&quot;1824.55&quot; x2=&quot;2008.13&quot; y2=&quot;1821.58&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2008.13&quot; y1=&quot;1801.47&quot; x2=&quot;2008.13&quot; y2=&quot;1798.63&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2008.13&quot; y1=&quot;1778.38&quot; x2=&quot;2008.13&quot; y2=&quot;1775.55&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2008.13&quot; y1=&quot;1755.30&quot; x2=&quot;2008.13&quot; y2=&quot;1752.46&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2008.13&quot; y1=&quot;1732.21&quot; x2=&quot;2008.13&quot; y2=&quot;1729.38&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2008.13&quot; y1=&quot;1709.26&quot; x2=&quot;2008.13&quot; y2=&quot;1706.29&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2008.13&quot; y1=&quot;1686.18&quot; x2=&quot;2008.13&quot; y2=&quot;1683.34&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2008.13&quot; y1=&quot;1663.09&quot; x2=&quot;2008.13&quot; y2=&quot;1660.26&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2008.13&quot; y1=&quot;1640.01&quot; x2=&quot;2008.13&quot; y2=&quot;1637.17&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2008.13&quot; y1=&quot;1616.92&quot; x2=&quot;2008.13&quot; y2=&quot;1614.09&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2008.13&quot; y1=&quot;1593.83&quot; x2=&quot;2008.13&quot; y2=&quot;1591.00&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2008.13&quot; y1=&quot;1570.88&quot; x2=&quot;2008.13&quot; y2=&quot;1567.91&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2008.13&quot; y1=&quot;1547.80&quot; x2=&quot;2008.13&quot; y2=&quot;1544.96&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2008.13&quot; y1=&quot;1524.71&quot; x2=&quot;2008.13&quot; y2=&quot;1521.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2008.13&quot; y1=&quot;1501.63&quot; x2=&quot;2008.13&quot; y2=&quot;1498.79&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2008.13&quot; y1=&quot;1478.54&quot; x2=&quot;2008.13&quot; y2=&quot;1475.71&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2008.13&quot; y1=&quot;1455.46&quot; x2=&quot;2008.13&quot; y2=&quot;1452.62&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2008.13&quot; y1=&quot;1432.51&quot; x2=&quot;2008.13&quot; y2=&quot;1429.54&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2008.13&quot; y1=&quot;1409.42&quot; x2=&quot;2008.13&quot; y2=&quot;1406.59&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2008.13&quot; y1=&quot;1386.34&quot; x2=&quot;2008.13&quot; y2=&quot;1383.50&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2008.13&quot; y1=&quot;1363.25&quot; x2=&quot;2008.13&quot; y2=&quot;1360.42&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2008.13&quot; y1=&quot;1340.17&quot; x2=&quot;2008.13&quot; y2=&quot;1337.33&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2008.13&quot; y1=&quot;1317.08&quot; x2=&quot;2008.13&quot; y2=&quot;1314.25&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2008.13&quot; y1=&quot;1294.13&quot; x2=&quot;2008.13&quot; y2=&quot;1291.16&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2008.13&quot; y1=&quot;1271.04&quot; x2=&quot;2008.13&quot; y2=&quot;1268.21&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2008.13&quot; y1=&quot;1247.96&quot; x2=&quot;2008.13&quot; y2=&quot;1245.12&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2008.13&quot; y1=&quot;1224.87&quot; x2=&quot;2008.13&quot; y2=&quot;1222.04&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2008.13&quot; y1=&quot;1201.79&quot; x2=&quot;2008.13&quot; y2=&quot;1198.95&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2008.13&quot; y1=&quot;1178.70&quot; x2=&quot;2008.13&quot; y2=&quot;1175.87&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2008.13&quot; y1=&quot;1155.75&quot; x2=&quot;2008.13&quot; y2=&quot;1152.78&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2008.13&quot; y1=&quot;1132.67&quot; x2=&quot;2008.13&quot; y2=&quot;1129.83&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2008.13&quot; y1=&quot;1109.58&quot; x2=&quot;2008.13&quot; y2=&quot;1106.75&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2008.13&quot; y1=&quot;1086.50&quot; x2=&quot;2008.13&quot; y2=&quot;1083.66&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2008.13&quot; y1=&quot;1063.41&quot; x2=&quot;2008.13&quot; y2=&quot;1060.58&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2008.13&quot; y1=&quot;1040.33&quot; x2=&quot;2008.13&quot; y2=&quot;1037.49&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2008.13&quot; y1=&quot;1017.38&quot; x2=&quot;2008.13&quot; y2=&quot;1014.41&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2008.13&quot; y1=&quot;994.29&quot; x2=&quot;2008.13&quot; y2=&quot;991.46&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2008.13&quot; y1=&quot;971.21&quot; x2=&quot;2008.13&quot; y2=&quot;968.37&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2008.13&quot; y1=&quot;948.12&quot; x2=&quot;2008.13&quot; y2=&quot;945.28&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2008.13&quot; y1=&quot;925.03&quot; x2=&quot;2008.13&quot; y2=&quot;922.20&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2008.13&quot; y1=&quot;901.95&quot; x2=&quot;2008.13&quot; y2=&quot;899.11&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2008.13&quot; y1=&quot;879.00&quot; x2=&quot;2008.13&quot; y2=&quot;876.03&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2008.13&quot; y1=&quot;855.91&quot; x2=&quot;2008.13&quot; y2=&quot;853.08&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2008.13&quot; y1=&quot;832.83&quot; x2=&quot;2008.13&quot; y2=&quot;829.99&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2008.13&quot; y1=&quot;809.74&quot; x2=&quot;2008.13&quot; y2=&quot;806.91&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2008.13&quot; y1=&quot;786.66&quot; x2=&quot;2008.13&quot; y2=&quot;783.82&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2008.13&quot; y1=&quot;763.57&quot; x2=&quot;2008.13&quot; y2=&quot;760.74&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2008.13&quot; y1=&quot;740.62&quot; x2=&quot;2008.13&quot; y2=&quot;737.65&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2008.13&quot; y1=&quot;717.54&quot; x2=&quot;2008.13&quot; y2=&quot;714.70&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2008.13&quot; y1=&quot;694.45&quot; x2=&quot;2008.13&quot; y2=&quot;691.62&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2008.13&quot; y1=&quot;671.37&quot; x2=&quot;2008.13&quot; y2=&quot;668.53&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2008.13&quot; y1=&quot;648.28&quot; x2=&quot;2008.13&quot; y2=&quot;645.45&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2008.13&quot; y1=&quot;625.19&quot; x2=&quot;2008.13&quot; y2=&quot;622.36&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2008.13&quot; y1=&quot;602.24&quot; x2=&quot;2008.13&quot; y2=&quot;599.27&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2008.13&quot; y1=&quot;579.16&quot; x2=&quot;2008.13&quot; y2=&quot;576.32&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2008.13&quot; y1=&quot;556.07&quot; x2=&quot;2008.13&quot; y2=&quot;553.24&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2008.13&quot; y1=&quot;532.99&quot; x2=&quot;2008.13&quot; y2=&quot;530.15&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2008.13&quot; y1=&quot;509.90&quot; x2=&quot;2008.13&quot; y2=&quot;507.07&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2008.13&quot; y1=&quot;486.82&quot; x2=&quot;2008.13&quot; y2=&quot;483.98&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2008.13&quot; y1=&quot;463.87&quot; x2=&quot;2008.13&quot; y2=&quot;460.90&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2008.13&quot; y1=&quot;440.78&quot; x2=&quot;2008.13&quot; y2=&quot;437.95&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2008.13&quot; y1=&quot;417.70&quot; x2=&quot;2008.13&quot; y2=&quot;414.86&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2008.13&quot; y1=&quot;394.61&quot; x2=&quot;2008.13&quot; y2=&quot;391.78&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2008.13&quot; y1=&quot;371.53&quot; x2=&quot;2008.13&quot; y2=&quot;368.69&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2008.13&quot; y1=&quot;348.44&quot; x2=&quot;2008.13&quot; y2=&quot;345.61&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2008.13&quot; y1=&quot;325.49&quot; x2=&quot;2008.13&quot; y2=&quot;322.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2008.13&quot; y1=&quot;302.40&quot; x2=&quot;2008.13&quot; y2=&quot;299.57&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2008.13&quot; y1=&quot;279.32&quot; x2=&quot;2008.13&quot; y2=&quot;276.48&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2008.13&quot; y1=&quot;256.23&quot; x2=&quot;2008.13&quot; y2=&quot;253.40&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;2608.65&quot; x2=&quot;2328.61&quot; y2=&quot;2605.81&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;2585.56&quot; x2=&quot;2328.61&quot; y2=&quot;2582.73&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;2562.48&quot; x2=&quot;2328.61&quot; y2=&quot;2559.64&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;2539.39&quot; x2=&quot;2328.61&quot; y2=&quot;2536.55&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;2516.44&quot; x2=&quot;2328.61&quot; y2=&quot;2513.47&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;2493.35&quot; x2=&quot;2328.61&quot; y2=&quot;2490.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;2470.27&quot; x2=&quot;2328.61&quot; y2=&quot;2467.43&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;2447.18&quot; x2=&quot;2328.61&quot; y2=&quot;2444.35&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;2424.10&quot; x2=&quot;2328.61&quot; y2=&quot;2421.26&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;2401.01&quot; x2=&quot;2328.61&quot; y2=&quot;2398.18&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;2378.06&quot; x2=&quot;2328.61&quot; y2=&quot;2375.09&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;2354.98&quot; x2=&quot;2328.61&quot; y2=&quot;2352.14&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;2331.89&quot; x2=&quot;2328.61&quot; y2=&quot;2329.06&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;2308.81&quot; x2=&quot;2328.61&quot; y2=&quot;2305.97&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;2285.72&quot; x2=&quot;2328.61&quot; y2=&quot;2282.89&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;2262.64&quot; x2=&quot;2328.61&quot; y2=&quot;2259.80&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;2239.68&quot; x2=&quot;2328.61&quot; y2=&quot;2236.71&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;2216.60&quot; x2=&quot;2328.61&quot; y2=&quot;2213.76&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;2193.51&quot; x2=&quot;2328.61&quot; y2=&quot;2190.68&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;2170.43&quot; x2=&quot;2328.61&quot; y2=&quot;2167.59&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;2147.34&quot; x2=&quot;2328.61&quot; y2=&quot;2144.51&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;2124.26&quot; x2=&quot;2328.61&quot; y2=&quot;2121.42&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;2101.31&quot; x2=&quot;2328.61&quot; y2=&quot;2098.34&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;2078.22&quot; x2=&quot;2328.61&quot; y2=&quot;2075.39&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;2055.14&quot; x2=&quot;2328.61&quot; y2=&quot;2052.30&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;2032.05&quot; x2=&quot;2328.61&quot; y2=&quot;2029.22&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;2008.97&quot; x2=&quot;2328.61&quot; y2=&quot;2006.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;1985.88&quot; x2=&quot;2328.61&quot; y2=&quot;1983.05&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;1962.93&quot; x2=&quot;2328.61&quot; y2=&quot;1959.96&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;1939.85&quot; x2=&quot;2328.61&quot; y2=&quot;1937.01&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;1916.76&quot; x2=&quot;2328.61&quot; y2=&quot;1913.92&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;1893.67&quot; x2=&quot;2328.61&quot; y2=&quot;1890.84&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;1870.59&quot; x2=&quot;2328.61&quot; y2=&quot;1867.75&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;1847.50&quot; x2=&quot;2328.61&quot; y2=&quot;1844.67&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;1824.55&quot; x2=&quot;2328.61&quot; y2=&quot;1821.58&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;1801.47&quot; x2=&quot;2328.61&quot; y2=&quot;1798.63&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;1778.38&quot; x2=&quot;2328.61&quot; y2=&quot;1775.55&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;1755.30&quot; x2=&quot;2328.61&quot; y2=&quot;1752.46&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;1732.21&quot; x2=&quot;2328.61&quot; y2=&quot;1729.38&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;1709.26&quot; x2=&quot;2328.61&quot; y2=&quot;1706.29&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;1686.18&quot; x2=&quot;2328.61&quot; y2=&quot;1683.34&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;1663.09&quot; x2=&quot;2328.61&quot; y2=&quot;1660.26&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;1640.01&quot; x2=&quot;2328.61&quot; y2=&quot;1637.17&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;1616.92&quot; x2=&quot;2328.61&quot; y2=&quot;1614.09&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;1593.83&quot; x2=&quot;2328.61&quot; y2=&quot;1591.00&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;1570.88&quot; x2=&quot;2328.61&quot; y2=&quot;1567.91&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;1547.80&quot; x2=&quot;2328.61&quot; y2=&quot;1544.96&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;1524.71&quot; x2=&quot;2328.61&quot; y2=&quot;1521.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;1501.63&quot; x2=&quot;2328.61&quot; y2=&quot;1498.79&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;1478.54&quot; x2=&quot;2328.61&quot; y2=&quot;1475.71&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;1455.46&quot; x2=&quot;2328.61&quot; y2=&quot;1452.62&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;1432.51&quot; x2=&quot;2328.61&quot; y2=&quot;1429.54&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;1409.42&quot; x2=&quot;2328.61&quot; y2=&quot;1406.59&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;1386.34&quot; x2=&quot;2328.61&quot; y2=&quot;1383.50&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;1363.25&quot; x2=&quot;2328.61&quot; y2=&quot;1360.42&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;1340.17&quot; x2=&quot;2328.61&quot; y2=&quot;1337.33&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;1317.08&quot; x2=&quot;2328.61&quot; y2=&quot;1314.25&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;1294.13&quot; x2=&quot;2328.61&quot; y2=&quot;1291.16&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;1271.04&quot; x2=&quot;2328.61&quot; y2=&quot;1268.21&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;1247.96&quot; x2=&quot;2328.61&quot; y2=&quot;1245.12&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;1224.87&quot; x2=&quot;2328.61&quot; y2=&quot;1222.04&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;1201.79&quot; x2=&quot;2328.61&quot; y2=&quot;1198.95&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;1178.70&quot; x2=&quot;2328.61&quot; y2=&quot;1175.87&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;1155.75&quot; x2=&quot;2328.61&quot; y2=&quot;1152.78&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;1132.67&quot; x2=&quot;2328.61&quot; y2=&quot;1129.83&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;1109.58&quot; x2=&quot;2328.61&quot; y2=&quot;1106.75&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;1086.50&quot; x2=&quot;2328.61&quot; y2=&quot;1083.66&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;1063.41&quot; x2=&quot;2328.61&quot; y2=&quot;1060.58&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;1040.33&quot; x2=&quot;2328.61&quot; y2=&quot;1037.49&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;1017.38&quot; x2=&quot;2328.61&quot; y2=&quot;1014.41&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;994.29&quot; x2=&quot;2328.61&quot; y2=&quot;991.46&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;971.21&quot; x2=&quot;2328.61&quot; y2=&quot;968.37&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;948.12&quot; x2=&quot;2328.61&quot; y2=&quot;945.28&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;925.03&quot; x2=&quot;2328.61&quot; y2=&quot;922.20&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;901.95&quot; x2=&quot;2328.61&quot; y2=&quot;899.11&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;879.00&quot; x2=&quot;2328.61&quot; y2=&quot;876.03&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;855.91&quot; x2=&quot;2328.61&quot; y2=&quot;853.08&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;832.83&quot; x2=&quot;2328.61&quot; y2=&quot;829.99&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;809.74&quot; x2=&quot;2328.61&quot; y2=&quot;806.91&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;786.66&quot; x2=&quot;2328.61&quot; y2=&quot;783.82&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;763.57&quot; x2=&quot;2328.61&quot; y2=&quot;760.74&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;740.62&quot; x2=&quot;2328.61&quot; y2=&quot;737.65&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;717.54&quot; x2=&quot;2328.61&quot; y2=&quot;714.70&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;694.45&quot; x2=&quot;2328.61&quot; y2=&quot;691.62&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;671.37&quot; x2=&quot;2328.61&quot; y2=&quot;668.53&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;648.28&quot; x2=&quot;2328.61&quot; y2=&quot;645.45&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;625.19&quot; x2=&quot;2328.61&quot; y2=&quot;622.36&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;602.24&quot; x2=&quot;2328.61&quot; y2=&quot;599.27&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;579.16&quot; x2=&quot;2328.61&quot; y2=&quot;576.32&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;556.07&quot; x2=&quot;2328.61&quot; y2=&quot;553.24&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;532.99&quot; x2=&quot;2328.61&quot; y2=&quot;530.15&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;509.90&quot; x2=&quot;2328.61&quot; y2=&quot;507.07&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;486.82&quot; x2=&quot;2328.61&quot; y2=&quot;483.98&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;463.87&quot; x2=&quot;2328.61&quot; y2=&quot;460.90&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;440.78&quot; x2=&quot;2328.61&quot; y2=&quot;437.95&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;417.70&quot; x2=&quot;2328.61&quot; y2=&quot;414.86&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;394.61&quot; x2=&quot;2328.61&quot; y2=&quot;391.78&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;371.53&quot; x2=&quot;2328.61&quot; y2=&quot;368.69&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;348.44&quot; x2=&quot;2328.61&quot; y2=&quot;345.61&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;325.49&quot; x2=&quot;2328.61&quot; y2=&quot;322.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;302.40&quot; x2=&quot;2328.61&quot; y2=&quot;299.57&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;279.32&quot; x2=&quot;2328.61&quot; y2=&quot;276.48&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;256.23&quot; x2=&quot;2328.61&quot; y2=&quot;253.40&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2649.11&quot; y1=&quot;2608.65&quot; x2=&quot;2649.11&quot; y2=&quot;2605.81&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2649.11&quot; y1=&quot;2585.56&quot; x2=&quot;2649.11&quot; y2=&quot;2582.73&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2649.11&quot; y1=&quot;2562.48&quot; x2=&quot;2649.11&quot; y2=&quot;2559.64&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2649.11&quot; y1=&quot;2539.39&quot; x2=&quot;2649.11&quot; y2=&quot;2536.55&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2649.11&quot; y1=&quot;2516.44&quot; x2=&quot;2649.11&quot; y2=&quot;2513.47&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2649.11&quot; y1=&quot;2493.35&quot; x2=&quot;2649.11&quot; y2=&quot;2490.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2649.11&quot; y1=&quot;2470.27&quot; x2=&quot;2649.11&quot; y2=&quot;2467.43&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2649.11&quot; y1=&quot;2447.18&quot; x2=&quot;2649.11&quot; y2=&quot;2444.35&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2649.11&quot; y1=&quot;2424.10&quot; x2=&quot;2649.11&quot; y2=&quot;2421.26&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2649.11&quot; y1=&quot;2401.01&quot; x2=&quot;2649.11&quot; y2=&quot;2398.18&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2649.11&quot; y1=&quot;2378.06&quot; x2=&quot;2649.11&quot; y2=&quot;2375.09&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2649.11&quot; y1=&quot;2354.98&quot; x2=&quot;2649.11&quot; y2=&quot;2352.14&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2649.11&quot; y1=&quot;2331.89&quot; x2=&quot;2649.11&quot; y2=&quot;2329.06&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2649.11&quot; y1=&quot;2308.81&quot; x2=&quot;2649.11&quot; y2=&quot;2305.97&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2649.11&quot; y1=&quot;2285.72&quot; x2=&quot;2649.11&quot; y2=&quot;2282.89&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2649.11&quot; y1=&quot;2262.64&quot; x2=&quot;2649.11&quot; y2=&quot;2259.80&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2649.11&quot; y1=&quot;2239.68&quot; x2=&quot;2649.11&quot; y2=&quot;2236.71&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2649.11&quot; y1=&quot;2216.60&quot; x2=&quot;2649.11&quot; y2=&quot;2213.76&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2649.11&quot; y1=&quot;2193.51&quot; x2=&quot;2649.11&quot; y2=&quot;2190.68&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2649.11&quot; y1=&quot;2170.43&quot; x2=&quot;2649.11&quot; y2=&quot;2167.59&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2649.11&quot; y1=&quot;2147.34&quot; x2=&quot;2649.11&quot; y2=&quot;2144.51&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2649.11&quot; y1=&quot;2124.26&quot; x2=&quot;2649.11&quot; y2=&quot;2121.42&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2649.11&quot; y1=&quot;2101.31&quot; x2=&quot;2649.11&quot; y2=&quot;2098.34&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2649.11&quot; y1=&quot;2078.22&quot; x2=&quot;2649.11&quot; y2=&quot;2075.39&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2649.11&quot; y1=&quot;2055.14&quot; x2=&quot;2649.11&quot; y2=&quot;2052.30&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2649.11&quot; y1=&quot;2032.05&quot; x2=&quot;2649.11&quot; y2=&quot;2029.22&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2649.11&quot; y1=&quot;2008.97&quot; x2=&quot;2649.11&quot; y2=&quot;2006.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2649.11&quot; y1=&quot;1985.88&quot; x2=&quot;2649.11&quot; y2=&quot;1983.05&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2649.11&quot; y1=&quot;1962.93&quot; x2=&quot;2649.11&quot; y2=&quot;1959.96&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2649.11&quot; y1=&quot;1939.85&quot; x2=&quot;2649.11&quot; y2=&quot;1937.01&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2649.11&quot; y1=&quot;1916.76&quot; x2=&quot;2649.11&quot; y2=&quot;1913.92&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2649.11&quot; y1=&quot;1893.67&quot; x2=&quot;2649.11&quot; y2=&quot;1890.84&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2649.11&quot; y1=&quot;1870.59&quot; x2=&quot;2649.11&quot; y2=&quot;1867.75&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2649.11&quot; y1=&quot;1847.50&quot; x2=&quot;2649.11&quot; y2=&quot;1844.67&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2649.11&quot; y1=&quot;1824.55&quot; x2=&quot;2649.11&quot; y2=&quot;1821.58&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2649.11&quot; y1=&quot;1801.47&quot; x2=&quot;2649.11&quot; y2=&quot;1798.63&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2649.11&quot; y1=&quot;1778.38&quot; x2=&quot;2649.11&quot; y2=&quot;1775.55&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2649.11&quot; y1=&quot;1755.30&quot; x2=&quot;2649.11&quot; y2=&quot;1752.46&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2649.11&quot; y1=&quot;1732.21&quot; x2=&quot;2649.11&quot; y2=&quot;1729.38&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2649.11&quot; y1=&quot;1709.26&quot; x2=&quot;2649.11&quot; y2=&quot;1706.29&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2649.11&quot; y1=&quot;1686.18&quot; x2=&quot;2649.11&quot; y2=&quot;1683.34&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2649.11&quot; y1=&quot;1663.09&quot; x2=&quot;2649.11&quot; y2=&quot;1660.26&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2649.11&quot; y1=&quot;1640.01&quot; x2=&quot;2649.11&quot; y2=&quot;1637.17&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2649.11&quot; y1=&quot;1616.92&quot; x2=&quot;2649.11&quot; y2=&quot;1614.09&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2649.11&quot; y1=&quot;1593.83&quot; x2=&quot;2649.11&quot; y2=&quot;1591.00&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2649.11&quot; y1=&quot;1570.88&quot; x2=&quot;2649.11&quot; y2=&quot;1567.91&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2649.11&quot; y1=&quot;1547.80&quot; x2=&quot;2649.11&quot; y2=&quot;1544.96&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2649.11&quot; y1=&quot;1524.71&quot; x2=&quot;2649.11&quot; y2=&quot;1521.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2649.11&quot; y1=&quot;1501.63&quot; x2=&quot;2649.11&quot; y2=&quot;1498.79&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2649.11&quot; y1=&quot;1478.54&quot; x2=&quot;2649.11&quot; y2=&quot;1475.71&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2649.11&quot; y1=&quot;1455.46&quot; x2=&quot;2649.11&quot; y2=&quot;1452.62&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2649.11&quot; y1=&quot;1432.51&quot; x2=&quot;2649.11&quot; y2=&quot;1429.54&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2649.11&quot; y1=&quot;1409.42&quot; x2=&quot;2649.11&quot; y2=&quot;1406.59&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2649.11&quot; y1=&quot;1386.34&quot; x2=&quot;2649.11&quot; y2=&quot;1383.50&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2649.11&quot; y1=&quot;1363.25&quot; x2=&quot;2649.11&quot; y2=&quot;1360.42&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2649.11&quot; y1=&quot;1340.17&quot; x2=&quot;2649.11&quot; y2=&quot;1337.33&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2649.11&quot; y1=&quot;1317.08&quot; x2=&quot;2649.11&quot; y2=&quot;1314.25&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2649.11&quot; y1=&quot;1294.13&quot; x2=&quot;2649.11&quot; y2=&quot;1291.16&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2649.11&quot; y1=&quot;1271.04&quot; x2=&quot;2649.11&quot; y2=&quot;1268.21&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2649.11&quot; y1=&quot;1247.96&quot; x2=&quot;2649.11&quot; y2=&quot;1245.12&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2649.11&quot; y1=&quot;1224.87&quot; x2=&quot;2649.11&quot; y2=&quot;1222.04&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2649.11&quot; y1=&quot;1201.79&quot; x2=&quot;2649.11&quot; y2=&quot;1198.95&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2649.11&quot; y1=&quot;1178.70&quot; x2=&quot;2649.11&quot; y2=&quot;1175.87&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2649.11&quot; y1=&quot;1155.75&quot; x2=&quot;2649.11&quot; y2=&quot;1152.78&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2649.11&quot; y1=&quot;1132.67&quot; x2=&quot;2649.11&quot; y2=&quot;1129.83&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2649.11&quot; y1=&quot;1109.58&quot; x2=&quot;2649.11&quot; y2=&quot;1106.75&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2649.11&quot; y1=&quot;1086.50&quot; x2=&quot;2649.11&quot; y2=&quot;1083.66&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2649.11&quot; y1=&quot;1063.41&quot; x2=&quot;2649.11&quot; y2=&quot;1060.58&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2649.11&quot; y1=&quot;1040.33&quot; x2=&quot;2649.11&quot; y2=&quot;1037.49&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2649.11&quot; y1=&quot;1017.38&quot; x2=&quot;2649.11&quot; y2=&quot;1014.41&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2649.11&quot; y1=&quot;994.29&quot; x2=&quot;2649.11&quot; y2=&quot;991.46&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2649.11&quot; y1=&quot;971.21&quot; x2=&quot;2649.11&quot; y2=&quot;968.37&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2649.11&quot; y1=&quot;948.12&quot; x2=&quot;2649.11&quot; y2=&quot;945.28&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2649.11&quot; y1=&quot;925.03&quot; x2=&quot;2649.11&quot; y2=&quot;922.20&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2649.11&quot; y1=&quot;901.95&quot; x2=&quot;2649.11&quot; y2=&quot;899.11&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2649.11&quot; y1=&quot;879.00&quot; x2=&quot;2649.11&quot; y2=&quot;876.03&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2649.11&quot; y1=&quot;855.91&quot; x2=&quot;2649.11&quot; y2=&quot;853.08&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2649.11&quot; y1=&quot;832.83&quot; x2=&quot;2649.11&quot; y2=&quot;829.99&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2649.11&quot; y1=&quot;809.74&quot; x2=&quot;2649.11&quot; y2=&quot;806.91&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2649.11&quot; y1=&quot;786.66&quot; x2=&quot;2649.11&quot; y2=&quot;783.82&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2649.11&quot; y1=&quot;763.57&quot; x2=&quot;2649.11&quot; y2=&quot;760.74&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2649.11&quot; y1=&quot;740.62&quot; x2=&quot;2649.11&quot; y2=&quot;737.65&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2649.11&quot; y1=&quot;717.54&quot; x2=&quot;2649.11&quot; y2=&quot;714.70&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2649.11&quot; y1=&quot;694.45&quot; x2=&quot;2649.11&quot; y2=&quot;691.62&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2649.11&quot; y1=&quot;671.37&quot; x2=&quot;2649.11&quot; y2=&quot;668.53&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2649.11&quot; y1=&quot;648.28&quot; x2=&quot;2649.11&quot; y2=&quot;645.45&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2649.11&quot; y1=&quot;625.19&quot; x2=&quot;2649.11&quot; y2=&quot;622.36&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2649.11&quot; y1=&quot;602.24&quot; x2=&quot;2649.11&quot; y2=&quot;599.27&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2649.11&quot; y1=&quot;579.16&quot; x2=&quot;2649.11&quot; y2=&quot;576.32&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2649.11&quot; y1=&quot;556.07&quot; x2=&quot;2649.11&quot; y2=&quot;553.24&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2649.11&quot; y1=&quot;532.99&quot; x2=&quot;2649.11&quot; y2=&quot;530.15&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2649.11&quot; y1=&quot;509.90&quot; x2=&quot;2649.11&quot; y2=&quot;507.07&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2649.11&quot; y1=&quot;486.82&quot; x2=&quot;2649.11&quot; y2=&quot;483.98&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2649.11&quot; y1=&quot;463.87&quot; x2=&quot;2649.11&quot; y2=&quot;460.90&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2649.11&quot; y1=&quot;440.78&quot; x2=&quot;2649.11&quot; y2=&quot;437.95&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2649.11&quot; y1=&quot;417.70&quot; x2=&quot;2649.11&quot; y2=&quot;414.86&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2649.11&quot; y1=&quot;394.61&quot; x2=&quot;2649.11&quot; y2=&quot;391.78&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2649.11&quot; y1=&quot;371.53&quot; x2=&quot;2649.11&quot; y2=&quot;368.69&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2649.11&quot; y1=&quot;348.44&quot; x2=&quot;2649.11&quot; y2=&quot;345.61&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2649.11&quot; y1=&quot;325.49&quot; x2=&quot;2649.11&quot; y2=&quot;322.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2649.11&quot; y1=&quot;302.40&quot; x2=&quot;2649.11&quot; y2=&quot;299.57&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2649.11&quot; y1=&quot;279.32&quot; x2=&quot;2649.11&quot; y2=&quot;276.48&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2649.11&quot; y1=&quot;256.23&quot; x2=&quot;2649.11&quot; y2=&quot;253.40&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;2608.65&quot; x2=&quot;2328.61&quot; y2=&quot;2585.56&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;2565.45&quot; x2=&quot;2328.61&quot; y2=&quot;2542.36&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;2522.24&quot; x2=&quot;2328.61&quot; y2=&quot;2499.16&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;2479.04&quot; x2=&quot;2328.61&quot; y2=&quot;2455.96&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;2435.84&quot; x2=&quot;2328.61&quot; y2=&quot;2412.76&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;2392.64&quot; x2=&quot;2328.61&quot; y2=&quot;2369.56&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;2349.44&quot; x2=&quot;2328.61&quot; y2=&quot;2326.36&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;2306.24&quot; x2=&quot;2328.61&quot; y2=&quot;2283.16&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;2263.04&quot; x2=&quot;2328.61&quot; y2=&quot;2239.95&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;2219.84&quot; x2=&quot;2328.61&quot; y2=&quot;2196.75&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;2176.64&quot; x2=&quot;2328.61&quot; y2=&quot;2153.55&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;2133.44&quot; x2=&quot;2328.61&quot; y2=&quot;2110.35&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;2090.24&quot; x2=&quot;2328.61&quot; y2=&quot;2067.15&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;2047.04&quot; x2=&quot;2328.61&quot; y2=&quot;2023.95&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;2003.84&quot; x2=&quot;2328.61&quot; y2=&quot;1980.75&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;1960.64&quot; x2=&quot;2328.61&quot; y2=&quot;1937.55&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;1917.43&quot; x2=&quot;2328.61&quot; y2=&quot;1894.35&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;1874.23&quot; x2=&quot;2328.61&quot; y2=&quot;1851.15&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;1831.03&quot; x2=&quot;2328.61&quot; y2=&quot;1807.95&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;1787.83&quot; x2=&quot;2328.61&quot; y2=&quot;1764.75&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;1744.63&quot; x2=&quot;2328.61&quot; y2=&quot;1721.55&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;1701.43&quot; x2=&quot;2328.61&quot; y2=&quot;1678.35&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;1658.23&quot; x2=&quot;2328.61&quot; y2=&quot;1635.15&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;1615.03&quot; x2=&quot;2328.61&quot; y2=&quot;1591.94&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;1571.83&quot; x2=&quot;2328.61&quot; y2=&quot;1548.74&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;1528.63&quot; x2=&quot;2328.61&quot; y2=&quot;1505.54&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;1485.43&quot; x2=&quot;2328.61&quot; y2=&quot;1462.34&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;1442.23&quot; x2=&quot;2328.61&quot; y2=&quot;1419.14&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;1399.03&quot; x2=&quot;2328.61&quot; y2=&quot;1375.94&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;1355.83&quot; x2=&quot;2328.61&quot; y2=&quot;1332.74&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;1312.63&quot; x2=&quot;2328.61&quot; y2=&quot;1289.54&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;1269.42&quot; x2=&quot;2328.61&quot; y2=&quot;1246.34&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;1226.22&quot; x2=&quot;2328.61&quot; y2=&quot;1203.14&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;1183.02&quot; x2=&quot;2328.61&quot; y2=&quot;1159.94&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;1139.82&quot; x2=&quot;2328.61&quot; y2=&quot;1116.74&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;1096.62&quot; x2=&quot;2328.61&quot; y2=&quot;1073.54&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;1053.42&quot; x2=&quot;2328.61&quot; y2=&quot;1030.34&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;1010.22&quot; x2=&quot;2328.61&quot; y2=&quot;987.14&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;967.02&quot; x2=&quot;2328.61&quot; y2=&quot;943.93&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;923.82&quot; x2=&quot;2328.61&quot; y2=&quot;900.73&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;880.62&quot; x2=&quot;2328.61&quot; y2=&quot;857.53&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;837.42&quot; x2=&quot;2328.61&quot; y2=&quot;814.33&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;794.22&quot; x2=&quot;2328.61&quot; y2=&quot;771.13&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;751.02&quot; x2=&quot;2328.61&quot; y2=&quot;727.93&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;707.82&quot; x2=&quot;2328.61&quot; y2=&quot;684.73&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;664.62&quot; x2=&quot;2328.61&quot; y2=&quot;641.53&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;621.41&quot; x2=&quot;2328.61&quot; y2=&quot;598.33&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;578.21&quot; x2=&quot;2328.61&quot; y2=&quot;555.13&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;535.01&quot; x2=&quot;2328.61&quot; y2=&quot;511.93&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;491.81&quot; x2=&quot;2328.61&quot; y2=&quot;468.73&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;448.61&quot; x2=&quot;2328.61&quot; y2=&quot;425.53&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;405.41&quot; x2=&quot;2328.61&quot; y2=&quot;382.33&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;362.21&quot; x2=&quot;2328.61&quot; y2=&quot;339.13&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;319.01&quot; x2=&quot;2328.61&quot; y2=&quot;295.92&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;275.81&quot; x2=&quot;2328.61&quot; y2=&quot;252.72&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;3092.58&quot; y1=&quot;642.88&quot; x2=&quot;3095.41&quot; y2=&quot;642.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3115.66&quot; y1=&quot;642.88&quot; x2=&quot;3118.50&quot; y2=&quot;642.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3138.75&quot; y1=&quot;642.88&quot; x2=&quot;3141.59&quot; y2=&quot;642.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3161.84&quot; y1=&quot;642.88&quot; x2=&quot;3164.67&quot; y2=&quot;642.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3184.78&quot; y1=&quot;642.88&quot; x2=&quot;3187.76&quot; y2=&quot;642.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3207.87&quot; y1=&quot;642.88&quot; x2=&quot;3210.70&quot; y2=&quot;642.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3230.95&quot; y1=&quot;642.88&quot; x2=&quot;3233.79&quot; y2=&quot;642.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3254.04&quot; y1=&quot;642.88&quot; x2=&quot;3256.88&quot; y2=&quot;642.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3277.13&quot; y1=&quot;642.88&quot; x2=&quot;3279.96&quot; y2=&quot;642.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3300.21&quot; y1=&quot;642.88&quot; x2=&quot;3303.05&quot; y2=&quot;642.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3323.16&quot; y1=&quot;642.88&quot; x2=&quot;3326.13&quot; y2=&quot;642.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3346.24&quot; y1=&quot;642.88&quot; x2=&quot;3349.08&quot; y2=&quot;642.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3369.33&quot; y1=&quot;642.88&quot; x2=&quot;3372.16&quot; y2=&quot;642.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3392.41&quot; y1=&quot;642.88&quot; x2=&quot;3395.25&quot; y2=&quot;642.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3415.50&quot; y1=&quot;642.88&quot; x2=&quot;3418.34&quot; y2=&quot;642.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3438.59&quot; y1=&quot;642.88&quot; x2=&quot;3441.42&quot; y2=&quot;642.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3461.53&quot; y1=&quot;642.88&quot; x2=&quot;3464.51&quot; y2=&quot;642.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3484.62&quot; y1=&quot;642.88&quot; x2=&quot;3487.45&quot; y2=&quot;642.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3507.70&quot; y1=&quot;642.88&quot; x2=&quot;3510.54&quot; y2=&quot;642.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3530.79&quot; y1=&quot;642.88&quot; x2=&quot;3533.63&quot; y2=&quot;642.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3553.88&quot; y1=&quot;642.88&quot; x2=&quot;3556.71&quot; y2=&quot;642.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3576.96&quot; y1=&quot;642.88&quot; x2=&quot;3579.80&quot; y2=&quot;642.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3599.91&quot; y1=&quot;642.88&quot; x2=&quot;3602.88&quot; y2=&quot;642.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3622.99&quot; y1=&quot;642.88&quot; x2=&quot;3625.83&quot; y2=&quot;642.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3646.08&quot; y1=&quot;642.88&quot; x2=&quot;3648.91&quot; y2=&quot;642.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3669.16&quot; y1=&quot;642.88&quot; x2=&quot;3672.00&quot; y2=&quot;642.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3692.25&quot; y1=&quot;642.88&quot; x2=&quot;3695.09&quot; y2=&quot;642.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3715.20&quot; y1=&quot;642.88&quot; x2=&quot;3718.17&quot; y2=&quot;642.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3738.28&quot; y1=&quot;642.88&quot; x2=&quot;3741.12&quot; y2=&quot;642.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3761.37&quot; y1=&quot;642.88&quot; x2=&quot;3764.20&quot; y2=&quot;642.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3784.45&quot; y1=&quot;642.88&quot; x2=&quot;3787.29&quot; y2=&quot;642.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3807.54&quot; y1=&quot;642.88&quot; x2=&quot;3810.38&quot; y2=&quot;642.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3830.63&quot; y1=&quot;642.88&quot; x2=&quot;3833.46&quot; y2=&quot;642.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3853.57&quot; y1=&quot;642.88&quot; x2=&quot;3856.55&quot; y2=&quot;642.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3876.66&quot; y1=&quot;642.88&quot; x2=&quot;3879.49&quot; y2=&quot;642.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3899.74&quot; y1=&quot;642.88&quot; x2=&quot;3902.58&quot; y2=&quot;642.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3922.83&quot; y1=&quot;642.88&quot; x2=&quot;3925.66&quot; y2=&quot;642.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3945.91&quot; y1=&quot;642.88&quot; x2=&quot;3948.75&quot; y2=&quot;642.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3969.00&quot; y1=&quot;642.88&quot; x2=&quot;3971.84&quot; y2=&quot;642.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3991.95&quot; y1=&quot;642.88&quot; x2=&quot;3994.92&quot; y2=&quot;642.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;4015.03&quot; y1=&quot;642.88&quot; x2=&quot;4017.87&quot; y2=&quot;642.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;4038.12&quot; y1=&quot;642.88&quot; x2=&quot;4040.95&quot; y2=&quot;642.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;4061.20&quot; y1=&quot;642.88&quot; x2=&quot;4064.04&quot; y2=&quot;642.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;4084.29&quot; y1=&quot;642.88&quot; x2=&quot;4087.13&quot; y2=&quot;642.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;4107.38&quot; y1=&quot;642.88&quot; x2=&quot;4110.21&quot; y2=&quot;642.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;4130.32&quot; y1=&quot;642.88&quot; x2=&quot;4133.30&quot; y2=&quot;642.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;4153.41&quot; y1=&quot;642.88&quot; x2=&quot;4156.24&quot; y2=&quot;642.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;4176.49&quot; y1=&quot;642.88&quot; x2=&quot;4179.33&quot; y2=&quot;642.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3092.58&quot; y1=&quot;1429.13&quot; x2=&quot;3095.41&quot; y2=&quot;1429.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3115.66&quot; y1=&quot;1429.13&quot; x2=&quot;3118.50&quot; y2=&quot;1429.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3138.75&quot; y1=&quot;1429.13&quot; x2=&quot;3141.59&quot; y2=&quot;1429.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3161.84&quot; y1=&quot;1429.13&quot; x2=&quot;3164.67&quot; y2=&quot;1429.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3184.78&quot; y1=&quot;1429.13&quot; x2=&quot;3187.76&quot; y2=&quot;1429.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3207.87&quot; y1=&quot;1429.13&quot; x2=&quot;3210.70&quot; y2=&quot;1429.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3230.95&quot; y1=&quot;1429.13&quot; x2=&quot;3233.79&quot; y2=&quot;1429.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3254.04&quot; y1=&quot;1429.13&quot; x2=&quot;3256.88&quot; y2=&quot;1429.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3277.13&quot; y1=&quot;1429.13&quot; x2=&quot;3279.96&quot; y2=&quot;1429.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3300.21&quot; y1=&quot;1429.13&quot; x2=&quot;3303.05&quot; y2=&quot;1429.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3323.16&quot; y1=&quot;1429.13&quot; x2=&quot;3326.13&quot; y2=&quot;1429.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3346.24&quot; y1=&quot;1429.13&quot; x2=&quot;3349.08&quot; y2=&quot;1429.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3369.33&quot; y1=&quot;1429.13&quot; x2=&quot;3372.16&quot; y2=&quot;1429.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3392.41&quot; y1=&quot;1429.13&quot; x2=&quot;3395.25&quot; y2=&quot;1429.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3415.50&quot; y1=&quot;1429.13&quot; x2=&quot;3418.34&quot; y2=&quot;1429.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3438.59&quot; y1=&quot;1429.13&quot; x2=&quot;3441.42&quot; y2=&quot;1429.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3461.53&quot; y1=&quot;1429.13&quot; x2=&quot;3464.51&quot; y2=&quot;1429.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3484.62&quot; y1=&quot;1429.13&quot; x2=&quot;3487.45&quot; y2=&quot;1429.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3507.70&quot; y1=&quot;1429.13&quot; x2=&quot;3510.54&quot; y2=&quot;1429.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3530.79&quot; y1=&quot;1429.13&quot; x2=&quot;3533.63&quot; y2=&quot;1429.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3553.88&quot; y1=&quot;1429.13&quot; x2=&quot;3556.71&quot; y2=&quot;1429.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3576.96&quot; y1=&quot;1429.13&quot; x2=&quot;3579.80&quot; y2=&quot;1429.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3599.91&quot; y1=&quot;1429.13&quot; x2=&quot;3602.88&quot; y2=&quot;1429.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3622.99&quot; y1=&quot;1429.13&quot; x2=&quot;3625.83&quot; y2=&quot;1429.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3646.08&quot; y1=&quot;1429.13&quot; x2=&quot;3648.91&quot; y2=&quot;1429.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3669.16&quot; y1=&quot;1429.13&quot; x2=&quot;3672.00&quot; y2=&quot;1429.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3692.25&quot; y1=&quot;1429.13&quot; x2=&quot;3695.09&quot; y2=&quot;1429.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3715.20&quot; y1=&quot;1429.13&quot; x2=&quot;3718.17&quot; y2=&quot;1429.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3738.28&quot; y1=&quot;1429.13&quot; x2=&quot;3741.12&quot; y2=&quot;1429.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3761.37&quot; y1=&quot;1429.13&quot; x2=&quot;3764.20&quot; y2=&quot;1429.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3784.45&quot; y1=&quot;1429.13&quot; x2=&quot;3787.29&quot; y2=&quot;1429.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3807.54&quot; y1=&quot;1429.13&quot; x2=&quot;3810.38&quot; y2=&quot;1429.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3830.63&quot; y1=&quot;1429.13&quot; x2=&quot;3833.46&quot; y2=&quot;1429.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3853.57&quot; y1=&quot;1429.13&quot; x2=&quot;3856.55&quot; y2=&quot;1429.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3876.66&quot; y1=&quot;1429.13&quot; x2=&quot;3879.49&quot; y2=&quot;1429.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3899.74&quot; y1=&quot;1429.13&quot; x2=&quot;3902.58&quot; y2=&quot;1429.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3922.83&quot; y1=&quot;1429.13&quot; x2=&quot;3925.66&quot; y2=&quot;1429.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3945.91&quot; y1=&quot;1429.13&quot; x2=&quot;3948.75&quot; y2=&quot;1429.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3969.00&quot; y1=&quot;1429.13&quot; x2=&quot;3971.84&quot; y2=&quot;1429.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3991.95&quot; y1=&quot;1429.13&quot; x2=&quot;3994.92&quot; y2=&quot;1429.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;4015.03&quot; y1=&quot;1429.13&quot; x2=&quot;4017.87&quot; y2=&quot;1429.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;4038.12&quot; y1=&quot;1429.13&quot; x2=&quot;4040.95&quot; y2=&quot;1429.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;4061.20&quot; y1=&quot;1429.13&quot; x2=&quot;4064.04&quot; y2=&quot;1429.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;4084.29&quot; y1=&quot;1429.13&quot; x2=&quot;4087.13&quot; y2=&quot;1429.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;4107.38&quot; y1=&quot;1429.13&quot; x2=&quot;4110.21&quot; y2=&quot;1429.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;4130.32&quot; y1=&quot;1429.13&quot; x2=&quot;4133.30&quot; y2=&quot;1429.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;4153.41&quot; y1=&quot;1429.13&quot; x2=&quot;4156.24&quot; y2=&quot;1429.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;4176.49&quot; y1=&quot;1429.13&quot; x2=&quot;4179.33&quot; y2=&quot;1429.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3092.58&quot; y1=&quot;2215.52&quot; x2=&quot;3095.41&quot; y2=&quot;2215.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3115.66&quot; y1=&quot;2215.52&quot; x2=&quot;3118.50&quot; y2=&quot;2215.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3138.75&quot; y1=&quot;2215.52&quot; x2=&quot;3141.59&quot; y2=&quot;2215.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3161.84&quot; y1=&quot;2215.52&quot; x2=&quot;3164.67&quot; y2=&quot;2215.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3184.78&quot; y1=&quot;2215.52&quot; x2=&quot;3187.76&quot; y2=&quot;2215.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3207.87&quot; y1=&quot;2215.52&quot; x2=&quot;3210.70&quot; y2=&quot;2215.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3230.95&quot; y1=&quot;2215.52&quot; x2=&quot;3233.79&quot; y2=&quot;2215.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3254.04&quot; y1=&quot;2215.52&quot; x2=&quot;3256.88&quot; y2=&quot;2215.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3277.13&quot; y1=&quot;2215.52&quot; x2=&quot;3279.96&quot; y2=&quot;2215.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3300.21&quot; y1=&quot;2215.52&quot; x2=&quot;3303.05&quot; y2=&quot;2215.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3323.16&quot; y1=&quot;2215.52&quot; x2=&quot;3326.13&quot; y2=&quot;2215.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3346.24&quot; y1=&quot;2215.52&quot; x2=&quot;3349.08&quot; y2=&quot;2215.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3369.33&quot; y1=&quot;2215.52&quot; x2=&quot;3372.16&quot; y2=&quot;2215.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3392.41&quot; y1=&quot;2215.52&quot; x2=&quot;3395.25&quot; y2=&quot;2215.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3415.50&quot; y1=&quot;2215.52&quot; x2=&quot;3418.34&quot; y2=&quot;2215.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3438.59&quot; y1=&quot;2215.52&quot; x2=&quot;3441.42&quot; y2=&quot;2215.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3461.53&quot; y1=&quot;2215.52&quot; x2=&quot;3464.51&quot; y2=&quot;2215.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3484.62&quot; y1=&quot;2215.52&quot; x2=&quot;3487.45&quot; y2=&quot;2215.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3507.70&quot; y1=&quot;2215.52&quot; x2=&quot;3510.54&quot; y2=&quot;2215.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3530.79&quot; y1=&quot;2215.52&quot; x2=&quot;3533.63&quot; y2=&quot;2215.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3553.88&quot; y1=&quot;2215.52&quot; x2=&quot;3556.71&quot; y2=&quot;2215.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3576.96&quot; y1=&quot;2215.52&quot; x2=&quot;3579.80&quot; y2=&quot;2215.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3599.91&quot; y1=&quot;2215.52&quot; x2=&quot;3602.88&quot; y2=&quot;2215.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3622.99&quot; y1=&quot;2215.52&quot; x2=&quot;3625.83&quot; y2=&quot;2215.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3646.08&quot; y1=&quot;2215.52&quot; x2=&quot;3648.91&quot; y2=&quot;2215.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3669.16&quot; y1=&quot;2215.52&quot; x2=&quot;3672.00&quot; y2=&quot;2215.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3692.25&quot; y1=&quot;2215.52&quot; x2=&quot;3695.09&quot; y2=&quot;2215.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3715.20&quot; y1=&quot;2215.52&quot; x2=&quot;3718.17&quot; y2=&quot;2215.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3738.28&quot; y1=&quot;2215.52&quot; x2=&quot;3741.12&quot; y2=&quot;2215.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3761.37&quot; y1=&quot;2215.52&quot; x2=&quot;3764.20&quot; y2=&quot;2215.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3784.45&quot; y1=&quot;2215.52&quot; x2=&quot;3787.29&quot; y2=&quot;2215.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3807.54&quot; y1=&quot;2215.52&quot; x2=&quot;3810.38&quot; y2=&quot;2215.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3830.63&quot; y1=&quot;2215.52&quot; x2=&quot;3833.46&quot; y2=&quot;2215.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3853.57&quot; y1=&quot;2215.52&quot; x2=&quot;3856.55&quot; y2=&quot;2215.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3876.66&quot; y1=&quot;2215.52&quot; x2=&quot;3879.49&quot; y2=&quot;2215.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3899.74&quot; y1=&quot;2215.52&quot; x2=&quot;3902.58&quot; y2=&quot;2215.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3922.83&quot; y1=&quot;2215.52&quot; x2=&quot;3925.66&quot; y2=&quot;2215.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3945.91&quot; y1=&quot;2215.52&quot; x2=&quot;3948.75&quot; y2=&quot;2215.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3969.00&quot; y1=&quot;2215.52&quot; x2=&quot;3971.84&quot; y2=&quot;2215.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3991.95&quot; y1=&quot;2215.52&quot; x2=&quot;3994.92&quot; y2=&quot;2215.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;4015.03&quot; y1=&quot;2215.52&quot; x2=&quot;4017.87&quot; y2=&quot;2215.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;4038.12&quot; y1=&quot;2215.52&quot; x2=&quot;4040.95&quot; y2=&quot;2215.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;4061.20&quot; y1=&quot;2215.52&quot; x2=&quot;4064.04&quot; y2=&quot;2215.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;4084.29&quot; y1=&quot;2215.52&quot; x2=&quot;4087.13&quot; y2=&quot;2215.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;4107.38&quot; y1=&quot;2215.52&quot; x2=&quot;4110.21&quot; y2=&quot;2215.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;4130.32&quot; y1=&quot;2215.52&quot; x2=&quot;4133.30&quot; y2=&quot;2215.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;4153.41&quot; y1=&quot;2215.52&quot; x2=&quot;4156.24&quot; y2=&quot;2215.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;4176.49&quot; y1=&quot;2215.52&quot; x2=&quot;4179.33&quot; y2=&quot;2215.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3266.32&quot; y1=&quot;2608.65&quot; x2=&quot;3266.32&quot; y2=&quot;2605.81&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3266.32&quot; y1=&quot;2585.56&quot; x2=&quot;3266.32&quot; y2=&quot;2582.73&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3266.32&quot; y1=&quot;2562.48&quot; x2=&quot;3266.32&quot; y2=&quot;2559.64&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3266.32&quot; y1=&quot;2539.39&quot; x2=&quot;3266.32&quot; y2=&quot;2536.55&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3266.32&quot; y1=&quot;2516.44&quot; x2=&quot;3266.32&quot; y2=&quot;2513.47&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3266.32&quot; y1=&quot;2493.35&quot; x2=&quot;3266.32&quot; y2=&quot;2490.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3266.32&quot; y1=&quot;2470.27&quot; x2=&quot;3266.32&quot; y2=&quot;2467.43&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3266.32&quot; y1=&quot;2447.18&quot; x2=&quot;3266.32&quot; y2=&quot;2444.35&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3266.32&quot; y1=&quot;2424.10&quot; x2=&quot;3266.32&quot; y2=&quot;2421.26&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3266.32&quot; y1=&quot;2401.01&quot; x2=&quot;3266.32&quot; y2=&quot;2398.18&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3266.32&quot; y1=&quot;2378.06&quot; x2=&quot;3266.32&quot; y2=&quot;2375.09&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3266.32&quot; y1=&quot;2354.98&quot; x2=&quot;3266.32&quot; y2=&quot;2352.14&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3266.32&quot; y1=&quot;2331.89&quot; x2=&quot;3266.32&quot; y2=&quot;2329.06&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3266.32&quot; y1=&quot;2308.81&quot; x2=&quot;3266.32&quot; y2=&quot;2305.97&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3266.32&quot; y1=&quot;2285.72&quot; x2=&quot;3266.32&quot; y2=&quot;2282.89&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3266.32&quot; y1=&quot;2262.64&quot; x2=&quot;3266.32&quot; y2=&quot;2259.80&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3266.32&quot; y1=&quot;2239.68&quot; x2=&quot;3266.32&quot; y2=&quot;2236.71&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3266.32&quot; y1=&quot;2216.60&quot; x2=&quot;3266.32&quot; y2=&quot;2213.76&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3266.32&quot; y1=&quot;2193.51&quot; x2=&quot;3266.32&quot; y2=&quot;2190.68&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3266.32&quot; y1=&quot;2170.43&quot; x2=&quot;3266.32&quot; y2=&quot;2167.59&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3266.32&quot; y1=&quot;2147.34&quot; x2=&quot;3266.32&quot; y2=&quot;2144.51&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3266.32&quot; y1=&quot;2124.26&quot; x2=&quot;3266.32&quot; y2=&quot;2121.42&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3266.32&quot; y1=&quot;2101.31&quot; x2=&quot;3266.32&quot; y2=&quot;2098.34&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3266.32&quot; y1=&quot;2078.22&quot; x2=&quot;3266.32&quot; y2=&quot;2075.39&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3266.32&quot; y1=&quot;2055.14&quot; x2=&quot;3266.32&quot; y2=&quot;2052.30&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3266.32&quot; y1=&quot;2032.05&quot; x2=&quot;3266.32&quot; y2=&quot;2029.22&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3266.32&quot; y1=&quot;2008.97&quot; x2=&quot;3266.32&quot; y2=&quot;2006.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3266.32&quot; y1=&quot;1985.88&quot; x2=&quot;3266.32&quot; y2=&quot;1983.05&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3266.32&quot; y1=&quot;1962.93&quot; x2=&quot;3266.32&quot; y2=&quot;1959.96&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3266.32&quot; y1=&quot;1939.85&quot; x2=&quot;3266.32&quot; y2=&quot;1937.01&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3266.32&quot; y1=&quot;1916.76&quot; x2=&quot;3266.32&quot; y2=&quot;1913.92&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3266.32&quot; y1=&quot;1893.67&quot; x2=&quot;3266.32&quot; y2=&quot;1890.84&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3266.32&quot; y1=&quot;1870.59&quot; x2=&quot;3266.32&quot; y2=&quot;1867.75&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3266.32&quot; y1=&quot;1847.50&quot; x2=&quot;3266.32&quot; y2=&quot;1844.67&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3266.32&quot; y1=&quot;1824.55&quot; x2=&quot;3266.32&quot; y2=&quot;1821.58&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3266.32&quot; y1=&quot;1801.47&quot; x2=&quot;3266.32&quot; y2=&quot;1798.63&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3266.32&quot; y1=&quot;1778.38&quot; x2=&quot;3266.32&quot; y2=&quot;1775.55&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3266.32&quot; y1=&quot;1755.30&quot; x2=&quot;3266.32&quot; y2=&quot;1752.46&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3266.32&quot; y1=&quot;1732.21&quot; x2=&quot;3266.32&quot; y2=&quot;1729.38&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3266.32&quot; y1=&quot;1709.26&quot; x2=&quot;3266.32&quot; y2=&quot;1706.29&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3266.32&quot; y1=&quot;1686.18&quot; x2=&quot;3266.32&quot; y2=&quot;1683.34&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3266.32&quot; y1=&quot;1663.09&quot; x2=&quot;3266.32&quot; y2=&quot;1660.26&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3266.32&quot; y1=&quot;1640.01&quot; x2=&quot;3266.32&quot; y2=&quot;1637.17&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3266.32&quot; y1=&quot;1616.92&quot; x2=&quot;3266.32&quot; y2=&quot;1614.09&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3266.32&quot; y1=&quot;1593.83&quot; x2=&quot;3266.32&quot; y2=&quot;1591.00&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3266.32&quot; y1=&quot;1570.88&quot; x2=&quot;3266.32&quot; y2=&quot;1567.91&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3266.32&quot; y1=&quot;1547.80&quot; x2=&quot;3266.32&quot; y2=&quot;1544.96&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3266.32&quot; y1=&quot;1524.71&quot; x2=&quot;3266.32&quot; y2=&quot;1521.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3266.32&quot; y1=&quot;1501.63&quot; x2=&quot;3266.32&quot; y2=&quot;1498.79&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3266.32&quot; y1=&quot;1478.54&quot; x2=&quot;3266.32&quot; y2=&quot;1475.71&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3266.32&quot; y1=&quot;1455.46&quot; x2=&quot;3266.32&quot; y2=&quot;1452.62&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3266.32&quot; y1=&quot;1432.51&quot; x2=&quot;3266.32&quot; y2=&quot;1429.54&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3266.32&quot; y1=&quot;1409.42&quot; x2=&quot;3266.32&quot; y2=&quot;1406.59&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3266.32&quot; y1=&quot;1386.34&quot; x2=&quot;3266.32&quot; y2=&quot;1383.50&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3266.32&quot; y1=&quot;1363.25&quot; x2=&quot;3266.32&quot; y2=&quot;1360.42&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3266.32&quot; y1=&quot;1340.17&quot; x2=&quot;3266.32&quot; y2=&quot;1337.33&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3266.32&quot; y1=&quot;1317.08&quot; x2=&quot;3266.32&quot; y2=&quot;1314.25&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3266.32&quot; y1=&quot;1294.13&quot; x2=&quot;3266.32&quot; y2=&quot;1291.16&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3266.32&quot; y1=&quot;1271.04&quot; x2=&quot;3266.32&quot; y2=&quot;1268.21&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3266.32&quot; y1=&quot;1247.96&quot; x2=&quot;3266.32&quot; y2=&quot;1245.12&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3266.32&quot; y1=&quot;1224.87&quot; x2=&quot;3266.32&quot; y2=&quot;1222.04&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3266.32&quot; y1=&quot;1201.79&quot; x2=&quot;3266.32&quot; y2=&quot;1198.95&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3266.32&quot; y1=&quot;1178.70&quot; x2=&quot;3266.32&quot; y2=&quot;1175.87&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3266.32&quot; y1=&quot;1155.75&quot; x2=&quot;3266.32&quot; y2=&quot;1152.78&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3266.32&quot; y1=&quot;1132.67&quot; x2=&quot;3266.32&quot; y2=&quot;1129.83&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3266.32&quot; y1=&quot;1109.58&quot; x2=&quot;3266.32&quot; y2=&quot;1106.75&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3266.32&quot; y1=&quot;1086.50&quot; x2=&quot;3266.32&quot; y2=&quot;1083.66&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3266.32&quot; y1=&quot;1063.41&quot; x2=&quot;3266.32&quot; y2=&quot;1060.58&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3266.32&quot; y1=&quot;1040.33&quot; x2=&quot;3266.32&quot; y2=&quot;1037.49&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3266.32&quot; y1=&quot;1017.38&quot; x2=&quot;3266.32&quot; y2=&quot;1014.41&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3266.32&quot; y1=&quot;994.29&quot; x2=&quot;3266.32&quot; y2=&quot;991.46&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3266.32&quot; y1=&quot;971.21&quot; x2=&quot;3266.32&quot; y2=&quot;968.37&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3266.32&quot; y1=&quot;948.12&quot; x2=&quot;3266.32&quot; y2=&quot;945.28&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3266.32&quot; y1=&quot;925.03&quot; x2=&quot;3266.32&quot; y2=&quot;922.20&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3266.32&quot; y1=&quot;901.95&quot; x2=&quot;3266.32&quot; y2=&quot;899.11&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3266.32&quot; y1=&quot;879.00&quot; x2=&quot;3266.32&quot; y2=&quot;876.03&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3266.32&quot; y1=&quot;855.91&quot; x2=&quot;3266.32&quot; y2=&quot;853.08&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3266.32&quot; y1=&quot;832.83&quot; x2=&quot;3266.32&quot; y2=&quot;829.99&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3266.32&quot; y1=&quot;809.74&quot; x2=&quot;3266.32&quot; y2=&quot;806.91&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3266.32&quot; y1=&quot;786.66&quot; x2=&quot;3266.32&quot; y2=&quot;783.82&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3266.32&quot; y1=&quot;763.57&quot; x2=&quot;3266.32&quot; y2=&quot;760.74&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3266.32&quot; y1=&quot;740.62&quot; x2=&quot;3266.32&quot; y2=&quot;737.65&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3266.32&quot; y1=&quot;717.54&quot; x2=&quot;3266.32&quot; y2=&quot;714.70&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3266.32&quot; y1=&quot;694.45&quot; x2=&quot;3266.32&quot; y2=&quot;691.62&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3266.32&quot; y1=&quot;671.37&quot; x2=&quot;3266.32&quot; y2=&quot;668.53&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3266.32&quot; y1=&quot;648.28&quot; x2=&quot;3266.32&quot; y2=&quot;645.45&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3266.32&quot; y1=&quot;625.19&quot; x2=&quot;3266.32&quot; y2=&quot;622.36&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3266.32&quot; y1=&quot;602.24&quot; x2=&quot;3266.32&quot; y2=&quot;599.27&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3266.32&quot; y1=&quot;579.16&quot; x2=&quot;3266.32&quot; y2=&quot;576.32&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3266.32&quot; y1=&quot;556.07&quot; x2=&quot;3266.32&quot; y2=&quot;553.24&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3266.32&quot; y1=&quot;532.99&quot; x2=&quot;3266.32&quot; y2=&quot;530.15&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3266.32&quot; y1=&quot;509.90&quot; x2=&quot;3266.32&quot; y2=&quot;507.07&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3266.32&quot; y1=&quot;486.82&quot; x2=&quot;3266.32&quot; y2=&quot;483.98&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3266.32&quot; y1=&quot;463.87&quot; x2=&quot;3266.32&quot; y2=&quot;460.90&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3266.32&quot; y1=&quot;440.78&quot; x2=&quot;3266.32&quot; y2=&quot;437.95&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3266.32&quot; y1=&quot;417.70&quot; x2=&quot;3266.32&quot; y2=&quot;414.86&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3266.32&quot; y1=&quot;394.61&quot; x2=&quot;3266.32&quot; y2=&quot;391.78&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3266.32&quot; y1=&quot;371.53&quot; x2=&quot;3266.32&quot; y2=&quot;368.69&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3266.32&quot; y1=&quot;348.44&quot; x2=&quot;3266.32&quot; y2=&quot;345.61&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3266.32&quot; y1=&quot;325.49&quot; x2=&quot;3266.32&quot; y2=&quot;322.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3266.32&quot; y1=&quot;302.40&quot; x2=&quot;3266.32&quot; y2=&quot;299.57&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3266.32&quot; y1=&quot;279.32&quot; x2=&quot;3266.32&quot; y2=&quot;276.48&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3266.32&quot; y1=&quot;256.23&quot; x2=&quot;3266.32&quot; y2=&quot;253.40&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3657.55&quot; y1=&quot;2608.65&quot; x2=&quot;3657.55&quot; y2=&quot;2605.81&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3657.55&quot; y1=&quot;2585.56&quot; x2=&quot;3657.55&quot; y2=&quot;2582.73&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3657.55&quot; y1=&quot;2562.48&quot; x2=&quot;3657.55&quot; y2=&quot;2559.64&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3657.55&quot; y1=&quot;2539.39&quot; x2=&quot;3657.55&quot; y2=&quot;2536.55&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3657.55&quot; y1=&quot;2516.44&quot; x2=&quot;3657.55&quot; y2=&quot;2513.47&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3657.55&quot; y1=&quot;2493.35&quot; x2=&quot;3657.55&quot; y2=&quot;2490.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3657.55&quot; y1=&quot;2470.27&quot; x2=&quot;3657.55&quot; y2=&quot;2467.43&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3657.55&quot; y1=&quot;2447.18&quot; x2=&quot;3657.55&quot; y2=&quot;2444.35&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3657.55&quot; y1=&quot;2424.10&quot; x2=&quot;3657.55&quot; y2=&quot;2421.26&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3657.55&quot; y1=&quot;2401.01&quot; x2=&quot;3657.55&quot; y2=&quot;2398.18&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3657.55&quot; y1=&quot;2378.06&quot; x2=&quot;3657.55&quot; y2=&quot;2375.09&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3657.55&quot; y1=&quot;2354.98&quot; x2=&quot;3657.55&quot; y2=&quot;2352.14&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3657.55&quot; y1=&quot;2331.89&quot; x2=&quot;3657.55&quot; y2=&quot;2329.06&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3657.55&quot; y1=&quot;2308.81&quot; x2=&quot;3657.55&quot; y2=&quot;2305.97&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3657.55&quot; y1=&quot;2285.72&quot; x2=&quot;3657.55&quot; y2=&quot;2282.89&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3657.55&quot; y1=&quot;2262.64&quot; x2=&quot;3657.55&quot; y2=&quot;2259.80&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3657.55&quot; y1=&quot;2239.68&quot; x2=&quot;3657.55&quot; y2=&quot;2236.71&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3657.55&quot; y1=&quot;2216.60&quot; x2=&quot;3657.55&quot; y2=&quot;2213.76&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3657.55&quot; y1=&quot;2193.51&quot; x2=&quot;3657.55&quot; y2=&quot;2190.68&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3657.55&quot; y1=&quot;2170.43&quot; x2=&quot;3657.55&quot; y2=&quot;2167.59&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3657.55&quot; y1=&quot;2147.34&quot; x2=&quot;3657.55&quot; y2=&quot;2144.51&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3657.55&quot; y1=&quot;2124.26&quot; x2=&quot;3657.55&quot; y2=&quot;2121.42&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3657.55&quot; y1=&quot;2101.31&quot; x2=&quot;3657.55&quot; y2=&quot;2098.34&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3657.55&quot; y1=&quot;2078.22&quot; x2=&quot;3657.55&quot; y2=&quot;2075.39&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3657.55&quot; y1=&quot;2055.14&quot; x2=&quot;3657.55&quot; y2=&quot;2052.30&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3657.55&quot; y1=&quot;2032.05&quot; x2=&quot;3657.55&quot; y2=&quot;2029.22&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3657.55&quot; y1=&quot;2008.97&quot; x2=&quot;3657.55&quot; y2=&quot;2006.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3657.55&quot; y1=&quot;1985.88&quot; x2=&quot;3657.55&quot; y2=&quot;1983.05&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3657.55&quot; y1=&quot;1962.93&quot; x2=&quot;3657.55&quot; y2=&quot;1959.96&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3657.55&quot; y1=&quot;1939.85&quot; x2=&quot;3657.55&quot; y2=&quot;1937.01&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3657.55&quot; y1=&quot;1916.76&quot; x2=&quot;3657.55&quot; y2=&quot;1913.92&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3657.55&quot; y1=&quot;1893.67&quot; x2=&quot;3657.55&quot; y2=&quot;1890.84&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3657.55&quot; y1=&quot;1870.59&quot; x2=&quot;3657.55&quot; y2=&quot;1867.75&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3657.55&quot; y1=&quot;1847.50&quot; x2=&quot;3657.55&quot; y2=&quot;1844.67&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3657.55&quot; y1=&quot;1824.55&quot; x2=&quot;3657.55&quot; y2=&quot;1821.58&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3657.55&quot; y1=&quot;1801.47&quot; x2=&quot;3657.55&quot; y2=&quot;1798.63&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3657.55&quot; y1=&quot;1778.38&quot; x2=&quot;3657.55&quot; y2=&quot;1775.55&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3657.55&quot; y1=&quot;1755.30&quot; x2=&quot;3657.55&quot; y2=&quot;1752.46&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3657.55&quot; y1=&quot;1732.21&quot; x2=&quot;3657.55&quot; y2=&quot;1729.38&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3657.55&quot; y1=&quot;1709.26&quot; x2=&quot;3657.55&quot; y2=&quot;1706.29&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3657.55&quot; y1=&quot;1686.18&quot; x2=&quot;3657.55&quot; y2=&quot;1683.34&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3657.55&quot; y1=&quot;1663.09&quot; x2=&quot;3657.55&quot; y2=&quot;1660.26&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3657.55&quot; y1=&quot;1640.01&quot; x2=&quot;3657.55&quot; y2=&quot;1637.17&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3657.55&quot; y1=&quot;1616.92&quot; x2=&quot;3657.55&quot; y2=&quot;1614.09&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3657.55&quot; y1=&quot;1593.83&quot; x2=&quot;3657.55&quot; y2=&quot;1591.00&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3657.55&quot; y1=&quot;1570.88&quot; x2=&quot;3657.55&quot; y2=&quot;1567.91&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3657.55&quot; y1=&quot;1547.80&quot; x2=&quot;3657.55&quot; y2=&quot;1544.96&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3657.55&quot; y1=&quot;1524.71&quot; x2=&quot;3657.55&quot; y2=&quot;1521.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3657.55&quot; y1=&quot;1501.63&quot; x2=&quot;3657.55&quot; y2=&quot;1498.79&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3657.55&quot; y1=&quot;1478.54&quot; x2=&quot;3657.55&quot; y2=&quot;1475.71&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3657.55&quot; y1=&quot;1455.46&quot; x2=&quot;3657.55&quot; y2=&quot;1452.62&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3657.55&quot; y1=&quot;1432.51&quot; x2=&quot;3657.55&quot; y2=&quot;1429.54&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3657.55&quot; y1=&quot;1409.42&quot; x2=&quot;3657.55&quot; y2=&quot;1406.59&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3657.55&quot; y1=&quot;1386.34&quot; x2=&quot;3657.55&quot; y2=&quot;1383.50&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3657.55&quot; y1=&quot;1363.25&quot; x2=&quot;3657.55&quot; y2=&quot;1360.42&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3657.55&quot; y1=&quot;1340.17&quot; x2=&quot;3657.55&quot; y2=&quot;1337.33&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3657.55&quot; y1=&quot;1317.08&quot; x2=&quot;3657.55&quot; y2=&quot;1314.25&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3657.55&quot; y1=&quot;1294.13&quot; x2=&quot;3657.55&quot; y2=&quot;1291.16&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3657.55&quot; y1=&quot;1271.04&quot; x2=&quot;3657.55&quot; y2=&quot;1268.21&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3657.55&quot; y1=&quot;1247.96&quot; x2=&quot;3657.55&quot; y2=&quot;1245.12&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3657.55&quot; y1=&quot;1224.87&quot; x2=&quot;3657.55&quot; y2=&quot;1222.04&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3657.55&quot; y1=&quot;1201.79&quot; x2=&quot;3657.55&quot; y2=&quot;1198.95&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3657.55&quot; y1=&quot;1178.70&quot; x2=&quot;3657.55&quot; y2=&quot;1175.87&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3657.55&quot; y1=&quot;1155.75&quot; x2=&quot;3657.55&quot; y2=&quot;1152.78&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3657.55&quot; y1=&quot;1132.67&quot; x2=&quot;3657.55&quot; y2=&quot;1129.83&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3657.55&quot; y1=&quot;1109.58&quot; x2=&quot;3657.55&quot; y2=&quot;1106.75&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3657.55&quot; y1=&quot;1086.50&quot; x2=&quot;3657.55&quot; y2=&quot;1083.66&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3657.55&quot; y1=&quot;1063.41&quot; x2=&quot;3657.55&quot; y2=&quot;1060.58&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3657.55&quot; y1=&quot;1040.33&quot; x2=&quot;3657.55&quot; y2=&quot;1037.49&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3657.55&quot; y1=&quot;1017.38&quot; x2=&quot;3657.55&quot; y2=&quot;1014.41&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3657.55&quot; y1=&quot;994.29&quot; x2=&quot;3657.55&quot; y2=&quot;991.46&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3657.55&quot; y1=&quot;971.21&quot; x2=&quot;3657.55&quot; y2=&quot;968.37&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3657.55&quot; y1=&quot;948.12&quot; x2=&quot;3657.55&quot; y2=&quot;945.28&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3657.55&quot; y1=&quot;925.03&quot; x2=&quot;3657.55&quot; y2=&quot;922.20&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3657.55&quot; y1=&quot;901.95&quot; x2=&quot;3657.55&quot; y2=&quot;899.11&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3657.55&quot; y1=&quot;879.00&quot; x2=&quot;3657.55&quot; y2=&quot;876.03&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3657.55&quot; y1=&quot;855.91&quot; x2=&quot;3657.55&quot; y2=&quot;853.08&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3657.55&quot; y1=&quot;832.83&quot; x2=&quot;3657.55&quot; y2=&quot;829.99&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3657.55&quot; y1=&quot;809.74&quot; x2=&quot;3657.55&quot; y2=&quot;806.91&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3657.55&quot; y1=&quot;786.66&quot; x2=&quot;3657.55&quot; y2=&quot;783.82&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3657.55&quot; y1=&quot;763.57&quot; x2=&quot;3657.55&quot; y2=&quot;760.74&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3657.55&quot; y1=&quot;740.62&quot; x2=&quot;3657.55&quot; y2=&quot;737.65&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3657.55&quot; y1=&quot;717.54&quot; x2=&quot;3657.55&quot; y2=&quot;714.70&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3657.55&quot; y1=&quot;694.45&quot; x2=&quot;3657.55&quot; y2=&quot;691.62&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3657.55&quot; y1=&quot;671.37&quot; x2=&quot;3657.55&quot; y2=&quot;668.53&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3657.55&quot; y1=&quot;648.28&quot; x2=&quot;3657.55&quot; y2=&quot;645.45&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3657.55&quot; y1=&quot;625.19&quot; x2=&quot;3657.55&quot; y2=&quot;622.36&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3657.55&quot; y1=&quot;602.24&quot; x2=&quot;3657.55&quot; y2=&quot;599.27&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3657.55&quot; y1=&quot;579.16&quot; x2=&quot;3657.55&quot; y2=&quot;576.32&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3657.55&quot; y1=&quot;556.07&quot; x2=&quot;3657.55&quot; y2=&quot;553.24&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3657.55&quot; y1=&quot;532.99&quot; x2=&quot;3657.55&quot; y2=&quot;530.15&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3657.55&quot; y1=&quot;509.90&quot; x2=&quot;3657.55&quot; y2=&quot;507.07&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3657.55&quot; y1=&quot;486.82&quot; x2=&quot;3657.55&quot; y2=&quot;483.98&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3657.55&quot; y1=&quot;463.87&quot; x2=&quot;3657.55&quot; y2=&quot;460.90&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3657.55&quot; y1=&quot;440.78&quot; x2=&quot;3657.55&quot; y2=&quot;437.95&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3657.55&quot; y1=&quot;417.70&quot; x2=&quot;3657.55&quot; y2=&quot;414.86&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3657.55&quot; y1=&quot;394.61&quot; x2=&quot;3657.55&quot; y2=&quot;391.78&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3657.55&quot; y1=&quot;371.53&quot; x2=&quot;3657.55&quot; y2=&quot;368.69&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3657.55&quot; y1=&quot;348.44&quot; x2=&quot;3657.55&quot; y2=&quot;345.61&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3657.55&quot; y1=&quot;325.49&quot; x2=&quot;3657.55&quot; y2=&quot;322.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3657.55&quot; y1=&quot;302.40&quot; x2=&quot;3657.55&quot; y2=&quot;299.57&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3657.55&quot; y1=&quot;279.32&quot; x2=&quot;3657.55&quot; y2=&quot;276.48&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;3657.55&quot; y1=&quot;256.23&quot; x2=&quot;3657.55&quot; y2=&quot;253.40&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;2608.65&quot; x2=&quot;4048.78&quot; y2=&quot;2605.81&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;2585.56&quot; x2=&quot;4048.78&quot; y2=&quot;2582.73&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;2562.48&quot; x2=&quot;4048.78&quot; y2=&quot;2559.64&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;2539.39&quot; x2=&quot;4048.78&quot; y2=&quot;2536.55&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;2516.44&quot; x2=&quot;4048.78&quot; y2=&quot;2513.47&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;2493.35&quot; x2=&quot;4048.78&quot; y2=&quot;2490.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;2470.27&quot; x2=&quot;4048.78&quot; y2=&quot;2467.43&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;2447.18&quot; x2=&quot;4048.78&quot; y2=&quot;2444.35&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;2424.10&quot; x2=&quot;4048.78&quot; y2=&quot;2421.26&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;2401.01&quot; x2=&quot;4048.78&quot; y2=&quot;2398.18&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;2378.06&quot; x2=&quot;4048.78&quot; y2=&quot;2375.09&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;2354.98&quot; x2=&quot;4048.78&quot; y2=&quot;2352.14&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;2331.89&quot; x2=&quot;4048.78&quot; y2=&quot;2329.06&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;2308.81&quot; x2=&quot;4048.78&quot; y2=&quot;2305.97&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;2285.72&quot; x2=&quot;4048.78&quot; y2=&quot;2282.89&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;2262.64&quot; x2=&quot;4048.78&quot; y2=&quot;2259.80&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;2239.68&quot; x2=&quot;4048.78&quot; y2=&quot;2236.71&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;2216.60&quot; x2=&quot;4048.78&quot; y2=&quot;2213.76&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;2193.51&quot; x2=&quot;4048.78&quot; y2=&quot;2190.68&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;2170.43&quot; x2=&quot;4048.78&quot; y2=&quot;2167.59&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;2147.34&quot; x2=&quot;4048.78&quot; y2=&quot;2144.51&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;2124.26&quot; x2=&quot;4048.78&quot; y2=&quot;2121.42&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;2101.31&quot; x2=&quot;4048.78&quot; y2=&quot;2098.34&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;2078.22&quot; x2=&quot;4048.78&quot; y2=&quot;2075.39&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;2055.14&quot; x2=&quot;4048.78&quot; y2=&quot;2052.30&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;2032.05&quot; x2=&quot;4048.78&quot; y2=&quot;2029.22&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;2008.97&quot; x2=&quot;4048.78&quot; y2=&quot;2006.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;1985.88&quot; x2=&quot;4048.78&quot; y2=&quot;1983.05&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;1962.93&quot; x2=&quot;4048.78&quot; y2=&quot;1959.96&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;1939.85&quot; x2=&quot;4048.78&quot; y2=&quot;1937.01&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;1916.76&quot; x2=&quot;4048.78&quot; y2=&quot;1913.92&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;1893.67&quot; x2=&quot;4048.78&quot; y2=&quot;1890.84&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;1870.59&quot; x2=&quot;4048.78&quot; y2=&quot;1867.75&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;1847.50&quot; x2=&quot;4048.78&quot; y2=&quot;1844.67&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;1824.55&quot; x2=&quot;4048.78&quot; y2=&quot;1821.58&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;1801.47&quot; x2=&quot;4048.78&quot; y2=&quot;1798.63&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;1778.38&quot; x2=&quot;4048.78&quot; y2=&quot;1775.55&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;1755.30&quot; x2=&quot;4048.78&quot; y2=&quot;1752.46&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;1732.21&quot; x2=&quot;4048.78&quot; y2=&quot;1729.38&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;1709.26&quot; x2=&quot;4048.78&quot; y2=&quot;1706.29&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;1686.18&quot; x2=&quot;4048.78&quot; y2=&quot;1683.34&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;1663.09&quot; x2=&quot;4048.78&quot; y2=&quot;1660.26&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;1640.01&quot; x2=&quot;4048.78&quot; y2=&quot;1637.17&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;1616.92&quot; x2=&quot;4048.78&quot; y2=&quot;1614.09&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;1593.83&quot; x2=&quot;4048.78&quot; y2=&quot;1591.00&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;1570.88&quot; x2=&quot;4048.78&quot; y2=&quot;1567.91&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;1547.80&quot; x2=&quot;4048.78&quot; y2=&quot;1544.96&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;1524.71&quot; x2=&quot;4048.78&quot; y2=&quot;1521.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;1501.63&quot; x2=&quot;4048.78&quot; y2=&quot;1498.79&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;1478.54&quot; x2=&quot;4048.78&quot; y2=&quot;1475.71&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;1455.46&quot; x2=&quot;4048.78&quot; y2=&quot;1452.62&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;1432.51&quot; x2=&quot;4048.78&quot; y2=&quot;1429.54&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;1409.42&quot; x2=&quot;4048.78&quot; y2=&quot;1406.59&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;1386.34&quot; x2=&quot;4048.78&quot; y2=&quot;1383.50&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;1363.25&quot; x2=&quot;4048.78&quot; y2=&quot;1360.42&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;1340.17&quot; x2=&quot;4048.78&quot; y2=&quot;1337.33&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;1317.08&quot; x2=&quot;4048.78&quot; y2=&quot;1314.25&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;1294.13&quot; x2=&quot;4048.78&quot; y2=&quot;1291.16&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;1271.04&quot; x2=&quot;4048.78&quot; y2=&quot;1268.21&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;1247.96&quot; x2=&quot;4048.78&quot; y2=&quot;1245.12&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;1224.87&quot; x2=&quot;4048.78&quot; y2=&quot;1222.04&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;1201.79&quot; x2=&quot;4048.78&quot; y2=&quot;1198.95&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;1178.70&quot; x2=&quot;4048.78&quot; y2=&quot;1175.87&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;1155.75&quot; x2=&quot;4048.78&quot; y2=&quot;1152.78&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;1132.67&quot; x2=&quot;4048.78&quot; y2=&quot;1129.83&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;1109.58&quot; x2=&quot;4048.78&quot; y2=&quot;1106.75&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;1086.50&quot; x2=&quot;4048.78&quot; y2=&quot;1083.66&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;1063.41&quot; x2=&quot;4048.78&quot; y2=&quot;1060.58&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;1040.33&quot; x2=&quot;4048.78&quot; y2=&quot;1037.49&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;1017.38&quot; x2=&quot;4048.78&quot; y2=&quot;1014.41&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;994.29&quot; x2=&quot;4048.78&quot; y2=&quot;991.46&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;971.21&quot; x2=&quot;4048.78&quot; y2=&quot;968.37&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;948.12&quot; x2=&quot;4048.78&quot; y2=&quot;945.28&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;925.03&quot; x2=&quot;4048.78&quot; y2=&quot;922.20&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;901.95&quot; x2=&quot;4048.78&quot; y2=&quot;899.11&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;879.00&quot; x2=&quot;4048.78&quot; y2=&quot;876.03&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;855.91&quot; x2=&quot;4048.78&quot; y2=&quot;853.08&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;832.83&quot; x2=&quot;4048.78&quot; y2=&quot;829.99&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;809.74&quot; x2=&quot;4048.78&quot; y2=&quot;806.91&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;786.66&quot; x2=&quot;4048.78&quot; y2=&quot;783.82&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;763.57&quot; x2=&quot;4048.78&quot; y2=&quot;760.74&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;740.62&quot; x2=&quot;4048.78&quot; y2=&quot;737.65&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;717.54&quot; x2=&quot;4048.78&quot; y2=&quot;714.70&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;694.45&quot; x2=&quot;4048.78&quot; y2=&quot;691.62&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;671.37&quot; x2=&quot;4048.78&quot; y2=&quot;668.53&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;648.28&quot; x2=&quot;4048.78&quot; y2=&quot;645.45&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;625.19&quot; x2=&quot;4048.78&quot; y2=&quot;622.36&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;602.24&quot; x2=&quot;4048.78&quot; y2=&quot;599.27&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;579.16&quot; x2=&quot;4048.78&quot; y2=&quot;576.32&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;556.07&quot; x2=&quot;4048.78&quot; y2=&quot;553.24&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;532.99&quot; x2=&quot;4048.78&quot; y2=&quot;530.15&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;509.90&quot; x2=&quot;4048.78&quot; y2=&quot;507.07&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;486.82&quot; x2=&quot;4048.78&quot; y2=&quot;483.98&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;463.87&quot; x2=&quot;4048.78&quot; y2=&quot;460.90&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;440.78&quot; x2=&quot;4048.78&quot; y2=&quot;437.95&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;417.70&quot; x2=&quot;4048.78&quot; y2=&quot;414.86&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;394.61&quot; x2=&quot;4048.78&quot; y2=&quot;391.78&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;371.53&quot; x2=&quot;4048.78&quot; y2=&quot;368.69&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;348.44&quot; x2=&quot;4048.78&quot; y2=&quot;345.61&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;325.49&quot; x2=&quot;4048.78&quot; y2=&quot;322.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;302.40&quot; x2=&quot;4048.78&quot; y2=&quot;299.57&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;279.32&quot; x2=&quot;4048.78&quot; y2=&quot;276.48&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;256.23&quot; x2=&quot;4048.78&quot; y2=&quot;253.40&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;2608.65&quot; x2=&quot;4048.78&quot; y2=&quot;2585.56&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;2565.45&quot; x2=&quot;4048.78&quot; y2=&quot;2542.36&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;2522.24&quot; x2=&quot;4048.78&quot; y2=&quot;2499.16&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;2479.04&quot; x2=&quot;4048.78&quot; y2=&quot;2455.96&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;2435.84&quot; x2=&quot;4048.78&quot; y2=&quot;2412.76&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;2392.64&quot; x2=&quot;4048.78&quot; y2=&quot;2369.56&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;2349.44&quot; x2=&quot;4048.78&quot; y2=&quot;2326.36&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;2306.24&quot; x2=&quot;4048.78&quot; y2=&quot;2283.16&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;2263.04&quot; x2=&quot;4048.78&quot; y2=&quot;2239.95&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;2219.84&quot; x2=&quot;4048.78&quot; y2=&quot;2196.75&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;2176.64&quot; x2=&quot;4048.78&quot; y2=&quot;2153.55&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;2133.44&quot; x2=&quot;4048.78&quot; y2=&quot;2110.35&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;2090.24&quot; x2=&quot;4048.78&quot; y2=&quot;2067.15&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;2047.04&quot; x2=&quot;4048.78&quot; y2=&quot;2023.95&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;2003.84&quot; x2=&quot;4048.78&quot; y2=&quot;1980.75&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;1960.64&quot; x2=&quot;4048.78&quot; y2=&quot;1937.55&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;1917.43&quot; x2=&quot;4048.78&quot; y2=&quot;1894.35&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;1874.23&quot; x2=&quot;4048.78&quot; y2=&quot;1851.15&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;1831.03&quot; x2=&quot;4048.78&quot; y2=&quot;1807.95&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;1787.83&quot; x2=&quot;4048.78&quot; y2=&quot;1764.75&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;1744.63&quot; x2=&quot;4048.78&quot; y2=&quot;1721.55&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;1701.43&quot; x2=&quot;4048.78&quot; y2=&quot;1678.35&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;1658.23&quot; x2=&quot;4048.78&quot; y2=&quot;1635.15&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;1615.03&quot; x2=&quot;4048.78&quot; y2=&quot;1591.94&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;1571.83&quot; x2=&quot;4048.78&quot; y2=&quot;1548.74&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;1528.63&quot; x2=&quot;4048.78&quot; y2=&quot;1505.54&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;1485.43&quot; x2=&quot;4048.78&quot; y2=&quot;1462.34&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;1442.23&quot; x2=&quot;4048.78&quot; y2=&quot;1419.14&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;1399.03&quot; x2=&quot;4048.78&quot; y2=&quot;1375.94&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;1355.83&quot; x2=&quot;4048.78&quot; y2=&quot;1332.74&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;1312.63&quot; x2=&quot;4048.78&quot; y2=&quot;1289.54&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;1269.42&quot; x2=&quot;4048.78&quot; y2=&quot;1246.34&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;1226.22&quot; x2=&quot;4048.78&quot; y2=&quot;1203.14&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;1183.02&quot; x2=&quot;4048.78&quot; y2=&quot;1159.94&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;1139.82&quot; x2=&quot;4048.78&quot; y2=&quot;1116.74&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;1096.62&quot; x2=&quot;4048.78&quot; y2=&quot;1073.54&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;1053.42&quot; x2=&quot;4048.78&quot; y2=&quot;1030.34&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;1010.22&quot; x2=&quot;4048.78&quot; y2=&quot;987.14&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;967.02&quot; x2=&quot;4048.78&quot; y2=&quot;943.93&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;923.82&quot; x2=&quot;4048.78&quot; y2=&quot;900.73&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;880.62&quot; x2=&quot;4048.78&quot; y2=&quot;857.53&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;837.42&quot; x2=&quot;4048.78&quot; y2=&quot;814.33&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;794.22&quot; x2=&quot;4048.78&quot; y2=&quot;771.13&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;751.02&quot; x2=&quot;4048.78&quot; y2=&quot;727.93&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;707.82&quot; x2=&quot;4048.78&quot; y2=&quot;684.73&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;664.62&quot; x2=&quot;4048.78&quot; y2=&quot;641.53&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;621.41&quot; x2=&quot;4048.78&quot; y2=&quot;598.33&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;578.21&quot; x2=&quot;4048.78&quot; y2=&quot;555.13&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;535.01&quot; x2=&quot;4048.78&quot; y2=&quot;511.93&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;491.81&quot; x2=&quot;4048.78&quot; y2=&quot;468.73&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;448.61&quot; x2=&quot;4048.78&quot; y2=&quot;425.53&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;405.41&quot; x2=&quot;4048.78&quot; y2=&quot;382.33&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;362.21&quot; x2=&quot;4048.78&quot; y2=&quot;339.13&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;319.01&quot; x2=&quot;4048.78&quot; y2=&quot;295.92&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;275.81&quot; x2=&quot;4048.78&quot; y2=&quot;252.72&quot; style=&quot;stroke:#707070;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;824.31&quot; y1=&quot;642.88&quot; x2=&quot;1143.99&quot; y2=&quot;642.88&quot; style=&quot;stroke:#606060;stroke-width:9.79&quot;/&gt;
	&lt;line x1=&quot;1143.99&quot; y1=&quot;672.58&quot; x2=&quot;1143.99&quot; y2=&quot;613.04&quot; stroke-linecap=&quot;round&quot; style=&quot;stroke:#606060;stroke-width:9.79&quot;/&gt;
	&lt;line x1=&quot;824.31&quot; y1=&quot;672.58&quot; x2=&quot;824.31&quot; y2=&quot;613.04&quot; stroke-linecap=&quot;round&quot; style=&quot;stroke:#606060;stroke-width:9.79&quot;/&gt;
	&lt;line x1=&quot;1285.88&quot; y1=&quot;1429.13&quot; x2=&quot;1606.77&quot; y2=&quot;1429.13&quot; style=&quot;stroke:#606060;stroke-width:9.79&quot;/&gt;
	&lt;line x1=&quot;1606.77&quot; y1=&quot;1458.83&quot; x2=&quot;1606.77&quot; y2=&quot;1399.30&quot; stroke-linecap=&quot;round&quot; style=&quot;stroke:#606060;stroke-width:9.79&quot;/&gt;
	&lt;line x1=&quot;1285.88&quot; y1=&quot;1458.83&quot; x2=&quot;1285.88&quot; y2=&quot;1399.30&quot; stroke-linecap=&quot;round&quot; style=&quot;stroke:#606060;stroke-width:9.79&quot;/&gt;
	&lt;line x1=&quot;1201.77&quot; y1=&quot;2215.52&quot; x2=&quot;1522.39&quot; y2=&quot;2215.52&quot; style=&quot;stroke:#606060;stroke-width:9.79&quot;/&gt;
	&lt;line x1=&quot;1522.39&quot; y1=&quot;2245.22&quot; x2=&quot;1522.39&quot; y2=&quot;2185.68&quot; stroke-linecap=&quot;round&quot; style=&quot;stroke:#606060;stroke-width:9.79&quot;/&gt;
	&lt;line x1=&quot;1201.77&quot; y1=&quot;2245.22&quot; x2=&quot;1201.77&quot; y2=&quot;2185.68&quot; stroke-linecap=&quot;round&quot; style=&quot;stroke:#606060;stroke-width:9.79&quot;/&gt;
	&lt;circle cx=&quot;984.15&quot; cy=&quot;642.88&quot; r=&quot;15.26&quot; style=&quot;fill:none;stroke:#606060;stroke-width:7.34&quot;/&gt;
	&lt;text x=&quot;984.15&quot; y=&quot;559.58&quot; style=&quot;fill:#000000&quot; text-anchor=&quot;middle&quot;&gt;
		&lt;tspan style=&quot;font-family:&#x27;Helvetica&#x27;;font-style:italic;font-size:68.04px&quot;&gt;b&lt;/tspan&gt;
		&lt;tspan style=&quot;font-family:&#x27;Helvetica&#x27;;font-size:68.04px&quot;&gt; = -0.36, &lt;/tspan&gt;
		&lt;tspan style=&quot;font-family:&#x27;Helvetica&#x27;;font-style:italic;font-size:68.04px&quot;&gt;p&lt;/tspan&gt;
		&lt;tspan style=&quot;font-family:&#x27;Helvetica&#x27;;font-size:68.04px&quot;&gt; = 0.01&lt;/tspan&gt;
	&lt;/text&gt;
	&lt;circle cx=&quot;1446.26&quot; cy=&quot;1429.13&quot; r=&quot;15.26&quot; style=&quot;fill:none;stroke:#606060;stroke-width:7.34&quot;/&gt;
	&lt;text x=&quot;1446.26&quot; y=&quot;1345.84&quot; style=&quot;fill:#000000&quot; text-anchor=&quot;middle&quot;&gt;
		&lt;tspan style=&quot;font-family:&#x27;Helvetica&#x27;;font-style:italic;font-size:68.04px&quot;&gt;b&lt;/tspan&gt;
		&lt;tspan style=&quot;font-family:&#x27;Helvetica&#x27;;font-size:68.04px&quot;&gt; = 0.40, &lt;/tspan&gt;
		&lt;tspan style=&quot;font-family:&#x27;Helvetica&#x27;;font-style:italic;font-size:68.04px&quot;&gt;p&lt;/tspan&gt;
		&lt;tspan style=&quot;font-family:&#x27;Helvetica&#x27;;font-size:68.04px&quot;&gt; = 0.00&lt;/tspan&gt;
	&lt;/text&gt;
	&lt;circle cx=&quot;1362.02&quot; cy=&quot;2215.52&quot; r=&quot;15.26&quot; style=&quot;fill:none;stroke:#606060;stroke-width:7.34&quot;/&gt;
	&lt;text x=&quot;1362.02&quot; y=&quot;2132.22&quot; style=&quot;fill:#000000&quot; text-anchor=&quot;middle&quot;&gt;
		&lt;tspan style=&quot;font-family:&#x27;Helvetica&#x27;;font-style:italic;font-size:68.04px&quot;&gt;b&lt;/tspan&gt;
		&lt;tspan style=&quot;font-family:&#x27;Helvetica&#x27;;font-size:68.04px&quot;&gt; = 0.26, &lt;/tspan&gt;
		&lt;tspan style=&quot;font-family:&#x27;Helvetica&#x27;;font-style:italic;font-size:68.04px&quot;&gt;p&lt;/tspan&gt;
		&lt;tspan style=&quot;font-family:&#x27;Helvetica&#x27;;font-size:68.04px&quot;&gt; = 0.05&lt;/tspan&gt;
	&lt;/text&gt;
	&lt;line x1=&quot;1985.31&quot; y1=&quot;642.88&quot; x2=&quot;2373.70&quot; y2=&quot;642.88&quot; style=&quot;stroke:#606060;stroke-width:9.79&quot;/&gt;
	&lt;line x1=&quot;2373.70&quot; y1=&quot;672.58&quot; x2=&quot;2373.70&quot; y2=&quot;613.04&quot; stroke-linecap=&quot;round&quot; style=&quot;stroke:#606060;stroke-width:9.79&quot;/&gt;
	&lt;line x1=&quot;1985.31&quot; y1=&quot;672.58&quot; x2=&quot;1985.31&quot; y2=&quot;613.04&quot; stroke-linecap=&quot;round&quot; style=&quot;stroke:#606060;stroke-width:9.79&quot;/&gt;
	&lt;line x1=&quot;2376.68&quot; y1=&quot;1429.13&quot; x2=&quot;2766.55&quot; y2=&quot;1429.13&quot; style=&quot;stroke:#606060;stroke-width:9.79&quot;/&gt;
	&lt;line x1=&quot;2766.55&quot; y1=&quot;1458.83&quot; x2=&quot;2766.55&quot; y2=&quot;1399.30&quot; stroke-linecap=&quot;round&quot; style=&quot;stroke:#606060;stroke-width:9.79&quot;/&gt;
	&lt;line x1=&quot;2376.68&quot; y1=&quot;1458.83&quot; x2=&quot;2376.68&quot; y2=&quot;1399.30&quot; stroke-linecap=&quot;round&quot; style=&quot;stroke:#606060;stroke-width:9.79&quot;/&gt;
	&lt;line x1=&quot;2238.16&quot; y1=&quot;2215.52&quot; x2=&quot;2627.64&quot; y2=&quot;2215.52&quot; style=&quot;stroke:#606060;stroke-width:9.79&quot;/&gt;
	&lt;line x1=&quot;2627.64&quot; y1=&quot;2245.22&quot; x2=&quot;2627.64&quot; y2=&quot;2185.68&quot; stroke-linecap=&quot;round&quot; style=&quot;stroke:#606060;stroke-width:9.79&quot;/&gt;
	&lt;line x1=&quot;2238.16&quot; y1=&quot;2245.22&quot; x2=&quot;2238.16&quot; y2=&quot;2185.68&quot; stroke-linecap=&quot;round&quot; style=&quot;stroke:#606060;stroke-width:9.79&quot;/&gt;
	&lt;circle cx=&quot;2179.57&quot; cy=&quot;642.88&quot; r=&quot;15.26&quot; style=&quot;fill:none;stroke:#606060;stroke-width:7.34&quot;/&gt;
	&lt;text x=&quot;2179.57&quot; y=&quot;559.58&quot; style=&quot;fill:#000000&quot; text-anchor=&quot;middle&quot;&gt;
		&lt;tspan style=&quot;font-family:&#x27;Helvetica&#x27;;font-style:italic;font-size:68.04px&quot;&gt;b&lt;/tspan&gt;
		&lt;tspan style=&quot;font-family:&#x27;Helvetica&#x27;;font-size:68.04px&quot;&gt; = -0.23, &lt;/tspan&gt;
		&lt;tspan style=&quot;font-family:&#x27;Helvetica&#x27;;font-style:italic;font-size:68.04px&quot;&gt;p&lt;/tspan&gt;
		&lt;tspan style=&quot;font-family:&#x27;Helvetica&#x27;;font-size:68.04px&quot;&gt; = 0.13&lt;/tspan&gt;
	&lt;/text&gt;
	&lt;circle cx=&quot;2571.61&quot; cy=&quot;1429.13&quot; r=&quot;15.26&quot; style=&quot;fill:none;stroke:#606060;stroke-width:7.34&quot;/&gt;
	&lt;text x=&quot;2571.61&quot; y=&quot;1345.84&quot; style=&quot;fill:#000000&quot; text-anchor=&quot;middle&quot;&gt;
		&lt;tspan style=&quot;font-family:&#x27;Helvetica&#x27;;font-style:italic;font-size:68.04px&quot;&gt;b&lt;/tspan&gt;
		&lt;tspan style=&quot;font-family:&#x27;Helvetica&#x27;;font-size:68.04px&quot;&gt; = 0.38, &lt;/tspan&gt;
		&lt;tspan style=&quot;font-family:&#x27;Helvetica&#x27;;font-style:italic;font-size:68.04px&quot;&gt;p&lt;/tspan&gt;
		&lt;tspan style=&quot;font-family:&#x27;Helvetica&#x27;;font-size:68.04px&quot;&gt; = 0.01&lt;/tspan&gt;
	&lt;/text&gt;
	&lt;circle cx=&quot;2432.84&quot; cy=&quot;2215.52&quot; r=&quot;15.26&quot; style=&quot;fill:none;stroke:#606060;stroke-width:7.34&quot;/&gt;
	&lt;text x=&quot;2432.84&quot; y=&quot;2132.22&quot; style=&quot;fill:#000000&quot; text-anchor=&quot;middle&quot;&gt;
		&lt;tspan style=&quot;font-family:&#x27;Helvetica&#x27;;font-style:italic;font-size:68.04px&quot;&gt;b&lt;/tspan&gt;
		&lt;tspan style=&quot;font-family:&#x27;Helvetica&#x27;;font-size:68.04px&quot;&gt; = 0.16, &lt;/tspan&gt;
		&lt;tspan style=&quot;font-family:&#x27;Helvetica&#x27;;font-style:italic;font-size:68.04px&quot;&gt;p&lt;/tspan&gt;
		&lt;tspan style=&quot;font-family:&#x27;Helvetica&#x27;;font-size:68.04px&quot;&gt; = 0.29&lt;/tspan&gt;
	&lt;/text&gt;
	&lt;line x1=&quot;3345.57&quot; y1=&quot;642.88&quot; x2=&quot;4130.73&quot; y2=&quot;642.88&quot; style=&quot;stroke:#606060;stroke-width:9.79&quot;/&gt;
	&lt;line x1=&quot;4130.73&quot; y1=&quot;672.58&quot; x2=&quot;4130.73&quot; y2=&quot;613.04&quot; stroke-linecap=&quot;round&quot; style=&quot;stroke:#606060;stroke-width:9.79&quot;/&gt;
	&lt;line x1=&quot;3345.57&quot; y1=&quot;672.58&quot; x2=&quot;3345.57&quot; y2=&quot;613.04&quot; stroke-linecap=&quot;round&quot; style=&quot;stroke:#606060;stroke-width:9.79&quot;/&gt;
	&lt;line x1=&quot;3146.45&quot; y1=&quot;1429.13&quot; x2=&quot;3934.44&quot; y2=&quot;1429.13&quot; style=&quot;stroke:#606060;stroke-width:9.79&quot;/&gt;
	&lt;line x1=&quot;3934.44&quot; y1=&quot;1458.83&quot; x2=&quot;3934.44&quot; y2=&quot;1399.30&quot; stroke-linecap=&quot;round&quot; style=&quot;stroke:#606060;stroke-width:9.79&quot;/&gt;
	&lt;line x1=&quot;3146.45&quot; y1=&quot;1458.83&quot; x2=&quot;3146.45&quot; y2=&quot;1399.30&quot; stroke-linecap=&quot;round&quot; style=&quot;stroke:#606060;stroke-width:9.79&quot;/&gt;
	&lt;line x1=&quot;3245.67&quot; y1=&quot;2215.52&quot; x2=&quot;4032.99&quot; y2=&quot;2215.52&quot; style=&quot;stroke:#606060;stroke-width:9.79&quot;/&gt;
	&lt;line x1=&quot;4032.99&quot; y1=&quot;2245.22&quot; x2=&quot;4032.99&quot; y2=&quot;2185.68&quot; stroke-linecap=&quot;round&quot; style=&quot;stroke:#606060;stroke-width:9.79&quot;/&gt;
	&lt;line x1=&quot;3245.67&quot; y1=&quot;2245.22&quot; x2=&quot;3245.67&quot; y2=&quot;2185.68&quot; stroke-linecap=&quot;round&quot; style=&quot;stroke:#606060;stroke-width:9.79&quot;/&gt;
	&lt;circle cx=&quot;3738.15&quot; cy=&quot;642.88&quot; r=&quot;15.26&quot; style=&quot;fill:none;stroke:#606060;stroke-width:7.34&quot;/&gt;
	&lt;text x=&quot;3738.15&quot; y=&quot;559.58&quot; style=&quot;fill:#000000&quot; text-anchor=&quot;middle&quot;&gt;
		&lt;tspan style=&quot;font-family:&#x27;Helvetica&#x27;;font-style:italic;font-size:68.04px&quot;&gt;b&lt;/tspan&gt;
		&lt;tspan style=&quot;font-family:&#x27;Helvetica&#x27;;font-size:68.04px&quot;&gt; = -0.16, &lt;/tspan&gt;
		&lt;tspan style=&quot;font-family:&#x27;Helvetica&#x27;;font-style:italic;font-size:68.04px&quot;&gt;p&lt;/tspan&gt;
		&lt;tspan style=&quot;font-family:&#x27;Helvetica&#x27;;font-size:68.04px&quot;&gt; = 0.12&lt;/tspan&gt;
	&lt;/text&gt;
	&lt;circle cx=&quot;3540.51&quot; cy=&quot;1429.13&quot; r=&quot;15.26&quot; style=&quot;fill:none;stroke:#606060;stroke-width:7.34&quot;/&gt;
	&lt;text x=&quot;3540.51&quot; y=&quot;1345.84&quot; style=&quot;fill:#000000&quot; text-anchor=&quot;middle&quot;&gt;
		&lt;tspan style=&quot;font-family:&#x27;Helvetica&#x27;;font-style:italic;font-size:68.04px&quot;&gt;b&lt;/tspan&gt;
		&lt;tspan style=&quot;font-family:&#x27;Helvetica&#x27;;font-size:68.04px&quot;&gt; = -0.26, &lt;/tspan&gt;
		&lt;tspan style=&quot;font-family:&#x27;Helvetica&#x27;;font-style:italic;font-size:68.04px&quot;&gt;p&lt;/tspan&gt;
		&lt;tspan style=&quot;font-family:&#x27;Helvetica&#x27;;font-size:68.04px&quot;&gt; = 0.01&lt;/tspan&gt;
	&lt;/text&gt;
	&lt;circle cx=&quot;3639.33&quot; cy=&quot;2215.52&quot; r=&quot;15.26&quot; style=&quot;fill:none;stroke:#606060;stroke-width:7.34&quot;/&gt;
	&lt;text x=&quot;3639.33&quot; y=&quot;2132.22&quot; style=&quot;fill:#000000&quot; text-anchor=&quot;middle&quot;&gt;
		&lt;tspan style=&quot;font-family:&#x27;Helvetica&#x27;;font-style:italic;font-size:68.04px&quot;&gt;b&lt;/tspan&gt;
		&lt;tspan style=&quot;font-family:&#x27;Helvetica&#x27;;font-size:68.04px&quot;&gt; = -0.21, &lt;/tspan&gt;
		&lt;tspan style=&quot;font-family:&#x27;Helvetica&#x27;;font-style:italic;font-size:68.04px&quot;&gt;p&lt;/tspan&gt;
		&lt;tspan style=&quot;font-family:&#x27;Helvetica&#x27;;font-size:68.04px&quot;&gt; = 0.04&lt;/tspan&gt;
	&lt;/text&gt;
	&lt;line x1=&quot;770.45&quot; y1=&quot;642.88&quot; x2=&quot;736.42&quot; y2=&quot;642.88&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;text x=&quot;719.41&quot; y=&quot;666.73&quot; style=&quot;font-family:&#x27;Helvetica&#x27;;font-size:68.04px;fill:#000000&quot; text-anchor=&quot;end&quot;&gt;Natural Origin&lt;/text&gt;
	&lt;line x1=&quot;770.45&quot; y1=&quot;1429.13&quot; x2=&quot;736.42&quot; y2=&quot;1429.13&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;text x=&quot;719.41&quot; y=&quot;1452.98&quot; style=&quot;font-family:&#x27;Helvetica&#x27;;font-size:68.04px;fill:#000000&quot; text-anchor=&quot;end&quot;&gt;Chinese Conspiracy&lt;/text&gt;
	&lt;line x1=&quot;770.45&quot; y1=&quot;2215.52&quot; x2=&quot;736.42&quot; y2=&quot;2215.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;text x=&quot;719.41&quot; y=&quot;2239.37&quot; style=&quot;font-family:&#x27;Helvetica&#x27;;font-size:68.04px;fill:#000000&quot; text-anchor=&quot;end&quot;&gt;Competitive Frame&lt;/text&gt;
	&lt;line x1=&quot;770.45&quot; y1=&quot;642.88&quot; x2=&quot;770.45&quot; y2=&quot;2215.52&quot; style=&quot;stroke:#A0A0A0;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;901.80&quot; y1=&quot;2608.65&quot; x2=&quot;901.80&quot; y2=&quot;2642.67&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;text x=&quot;901.80&quot; y=&quot;2707.24&quot; style=&quot;font-family:&#x27;Helvetica&#x27;;font-size:68.04px;fill:#000000&quot; text-anchor=&quot;middle&quot;&gt;-.5&lt;/text&gt;
	&lt;line x1=&quot;1204.07&quot; y1=&quot;2608.65&quot; x2=&quot;1204.07&quot; y2=&quot;2642.67&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;text x=&quot;1204.07&quot; y=&quot;2707.24&quot; style=&quot;font-family:&#x27;Helvetica&#x27;;font-size:68.04px;fill:#000000&quot; text-anchor=&quot;middle&quot;&gt;0&lt;/text&gt;
	&lt;line x1=&quot;1506.33&quot; y1=&quot;2608.65&quot; x2=&quot;1506.33&quot; y2=&quot;2642.67&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;text x=&quot;1506.33&quot; y=&quot;2707.24&quot; style=&quot;font-family:&#x27;Helvetica&#x27;;font-size:68.04px;fill:#000000&quot; text-anchor=&quot;middle&quot;&gt;.5&lt;/text&gt;
	&lt;line x1=&quot;1808.60&quot; y1=&quot;2608.65&quot; x2=&quot;1808.60&quot; y2=&quot;2642.67&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;text x=&quot;1808.60&quot; y=&quot;2707.24&quot; style=&quot;font-family:&#x27;Helvetica&#x27;;font-size:68.04px;fill:#000000&quot; text-anchor=&quot;middle&quot;&gt;1&lt;/text&gt;
	&lt;line x1=&quot;901.80&quot; y1=&quot;2608.65&quot; x2=&quot;1808.60&quot; y2=&quot;2608.65&quot; style=&quot;stroke:#A0A0A0;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;2008.13&quot; y1=&quot;2608.65&quot; x2=&quot;2008.13&quot; y2=&quot;2642.67&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;text x=&quot;2008.13&quot; y=&quot;2707.24&quot; style=&quot;font-family:&#x27;Helvetica&#x27;;font-size:68.04px;fill:#000000&quot; text-anchor=&quot;middle&quot;&gt;-.5&lt;/text&gt;
	&lt;line x1=&quot;2328.61&quot; y1=&quot;2608.65&quot; x2=&quot;2328.61&quot; y2=&quot;2642.67&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;text x=&quot;2328.61&quot; y=&quot;2707.24&quot; style=&quot;font-family:&#x27;Helvetica&#x27;;font-size:68.04px;fill:#000000&quot; text-anchor=&quot;middle&quot;&gt;0&lt;/text&gt;
	&lt;line x1=&quot;2649.11&quot; y1=&quot;2608.65&quot; x2=&quot;2649.11&quot; y2=&quot;2642.67&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;text x=&quot;2649.11&quot; y=&quot;2707.24&quot; style=&quot;font-family:&#x27;Helvetica&#x27;;font-size:68.04px;fill:#000000&quot; text-anchor=&quot;middle&quot;&gt;.5&lt;/text&gt;
	&lt;line x1=&quot;2969.59&quot; y1=&quot;2608.65&quot; x2=&quot;2969.59&quot; y2=&quot;2642.67&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;text x=&quot;2969.59&quot; y=&quot;2707.24&quot; style=&quot;font-family:&#x27;Helvetica&#x27;;font-size:68.04px;fill:#000000&quot; text-anchor=&quot;middle&quot;&gt;1&lt;/text&gt;
	&lt;line x1=&quot;2008.13&quot; y1=&quot;2608.65&quot; x2=&quot;2969.59&quot; y2=&quot;2608.65&quot; style=&quot;stroke:#A0A0A0;stroke-width:6.12&quot;/&gt;
	&lt;line x1=&quot;3266.32&quot; y1=&quot;2608.65&quot; x2=&quot;3266.32&quot; y2=&quot;2642.67&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;text x=&quot;3266.32&quot; y=&quot;2707.24&quot; style=&quot;font-family:&#x27;Helvetica&#x27;;font-size:68.04px;fill:#000000&quot; text-anchor=&quot;middle&quot;&gt;-.4&lt;/text&gt;
	&lt;line x1=&quot;3657.55&quot; y1=&quot;2608.65&quot; x2=&quot;3657.55&quot; y2=&quot;2642.67&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;text x=&quot;3657.55&quot; y=&quot;2707.24&quot; style=&quot;font-family:&#x27;Helvetica&#x27;;font-size:68.04px;fill:#000000&quot; text-anchor=&quot;middle&quot;&gt;-.2&lt;/text&gt;
	&lt;line x1=&quot;4048.78&quot; y1=&quot;2608.65&quot; x2=&quot;4048.78&quot; y2=&quot;2642.67&quot; style=&quot;stroke:#A0A0A0;stroke-width:4.90&quot;/&gt;
	&lt;text x=&quot;4048.78&quot; y=&quot;2707.24&quot; style=&quot;font-family:&#x27;Helvetica&#x27;;font-size:68.04px;fill:#000000&quot; text-anchor=&quot;middle&quot;&gt;0&lt;/text&gt;
	&lt;line x1=&quot;3266.32&quot; y1=&quot;2608.65&quot; x2=&quot;4048.78&quot; y2=&quot;2608.65&quot; style=&quot;stroke:#A0A0A0;stroke-width:6.12&quot;/&gt;
	&lt;rect x=&quot;770.45&quot; y=&quot;135.41&quot; width=&quot;1091.88&quot; height=&quot;114.35&quot; style=&quot;fill:#D0D0D0&quot;/&gt;
	&lt;rect x=&quot;772.89&quot; y=&quot;137.86&quot; width=&quot;1086.98&quot; height=&quot;109.45&quot; style=&quot;fill:none;stroke:#000000;stroke-width:4.90&quot;/&gt;
	&lt;text x=&quot;1316.38&quot; y=&quot;209.46&quot; style=&quot;font-family:&#x27;Helvetica&#x27;;font-size:85.05px;fill:#000000&quot; text-anchor=&quot;middle&quot;&gt;Origin Beliefs&lt;/text&gt;
	&lt;rect x=&quot;1931.44&quot; y=&quot;135.41&quot; width=&quot;1092.02&quot; height=&quot;114.35&quot; style=&quot;fill:#D0D0D0&quot;/&gt;
	&lt;rect x=&quot;1933.89&quot; y=&quot;137.86&quot; width=&quot;1087.12&quot; height=&quot;109.45&quot; style=&quot;fill:none;stroke:#000000;stroke-width:4.90&quot;/&gt;
	&lt;text x=&quot;2477.52&quot; y=&quot;209.46&quot; style=&quot;font-family:&#x27;Helvetica&#x27;;font-size:85.05px;fill:#000000&quot; text-anchor=&quot;middle&quot;&gt;Bioweapon&lt;/text&gt;
	&lt;rect x=&quot;3092.58&quot; y=&quot;135.41&quot; width=&quot;1092.02&quot; height=&quot;114.35&quot; style=&quot;fill:#D0D0D0&quot;/&gt;
	&lt;rect x=&quot;3095.03&quot; y=&quot;137.86&quot; width=&quot;1087.12&quot; height=&quot;109.45&quot; style=&quot;fill:none;stroke:#000000;stroke-width:4.90&quot;/&gt;
	&lt;text x=&quot;3638.66&quot; y=&quot;209.46&quot; style=&quot;font-family:&#x27;Helvetica&#x27;;font-size:85.05px;fill:#000000&quot; text-anchor=&quot;middle&quot;&gt;Pro-Social Behavior&lt;/text&gt;
&lt;/svg&gt;
</body></html>"></iframe>



    
    
    (file Main_Effects.svg written in SVG format)
    
    (file Main_Effects.pdf written in PDF format)
    
    (file Main_Effects.png written in PNG format)
    

#### A List of Things to Complete

1. Alpha code and test for scaled variables
2. Full code to estimate models for entire set of dependent variables.
3. Make additional figure formats to display results.
4. Add to Appendix file.
