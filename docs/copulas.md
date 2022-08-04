
## Copulas

If you are familiar with how to [predict](https://microprediction.github.io/microprediction/predict.html) and
you have noticed the [zstreams](https://microprediction.github.io/microprediction/zstreams.html) that are created, you
might consider some more advanced functionality. The /copula API serves two purposes:

 - Simultaneous publication of multiple streams
 - Simultaneous publication of multiple streams with associated z2 or z3 `copula' streams 

The following family of streams illustrates the pattern.
    
     
| Type of stream           | Examples                                                                                                                                            |
|--------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------|
| Regular #1               | [c5_bitcoin.json](https://www.microprediction.org/stream_dashboard.html?stream=c5_bitcoin)                                                          |
| Regular #2               | [c5_ethereum.json](https://www.microprediction.org/stream_dashboard.html?stream=c5_ethereum)                                                        |
| Regular #3               | [c5_cardano.json](https://www.microprediction.org/stream_dashboard.html?stream=c5_cardano)                                                          |
| Z-scores stream #1       | [z1~c5_ethereum~3555](https://www.microprediction.org/stream_dashboard.html?stream=z1~c5_ethereum~3555)                                             |
| Bivariate copula #1,#2   | [z2~c5_bitcoin~c5_ethereum~3555](https://www.microprediction.org/stream_dashboard.html?stream=z2~c5_bitcoin~c5_ethereum~3555)                       |
| Trivariate copula (70s)  | [z3~c5_bitcoin~c5_cardano~c5_ethereum~70](https://www.microprediction.org/stream_dashboard.html?stream=z3~c5_bitcoin~c5_cardano~c5_ethereum~70)     |         
| Trivariate copula (1 hr) | [z3~c5_bitcoin~c5_cardano~c5_ethereum~3555](https://www.microprediction.org/stream_dashboard.html?stream=z3~c5_bitcoin~c5_cardano~c5_ethereum~3555) |         


To reiterate there are NO MULTIVARIATE streams on microprediction.org. However there are
univariate streams whose values represent a point in R^2 or R^3. The mapping from one dimension to 
two or three is implied. The former is used by streams prefixed by z2~, the latter by streams prefixed by
z3~.

### Python 
Very similar to the use of set()

    from microprediction import MicroWriter
    mw = MicroWriter(write_key='YOUR WRITE KEY HERE')
    names = ['my_stream_a.json','my_stream_b.json','my_stream_c.json', 'my_stream_d.json']
    values = [5,6,4,9]
    res = mw.cset(names=names,values=values)

### Definition
Probably you've already grok'd [zscores](https://microprediction.github.io/microprediction/zscores.html), and the bivariate and trivariate counterparts
are not $$y=x^2$$ dissimilar:

\( z_2 = 17 \)

Or this 

$$x^2 + 3$$


Morton space filling Z-curves are employed. The most up to date
reference for these embeddings is the code (see [zcurve_conventions](https://github.com/microprediction/microconventions/blob/master/microconventions/zcurve_conventions.py). At time of
writing these docs, the heart of this calculation is:

    pymorton.interleave(prctls)

where 'prctls' are the 
 