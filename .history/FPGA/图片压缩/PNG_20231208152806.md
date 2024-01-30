understanding PNG from the perspective of Lossless Compression
![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/undefinedPasted%20image%2020230706214132.png)

**Exploit Redundancy and Huffman codes**
![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/undefinedPasted%20image%2020230707094027.png)

## Filtering 

![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/undefinedPasted%20image%2020230707090619.png)

the way PNG solves this problem involves a pre-procesing step call **filtering**

in this particular image,the difference between every adjacent pixel is four,if we transform the row to store difference rather than the raw LZSS can perform significantly better

every filter actually operates on individual bytes not necessarily pixels
### Five Filters

#### NONE

![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/undefinedPasted%20image%2020230707091451.png)

#### SUB

![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/undefinedPasted%20image%2020230707091437.png)

take the difference and apply a mod 256 operation

$$
X_{out}=(X-L)\%256
$$


#### UP

![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/undefinedPasted%20image%2020230707091843.png)

#### AVG

![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/undefinedPasted%20image%2020230707092032.png)

#### PAETH

![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/undefinedPasted%20image%2020230707092227.png)

####

because we are dealing with LR and U pixels edge cases in a very  literal sense do come in -->find ant out of bounds pixel as zero

![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/undefinedPasted%20image%2020230707092542.png)


### How PNG even figures out what filter to use?

Minimum Sum of absolute differences

![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/undefinedPasted%20image%2020230707093101.png)

$$
Score=\sum_{i=1}^n|x_i|
$$

what PNG does to pick the best filter is perform this calculation on all five filters for each row,and then pick the filter that has the minimum score

###

on the decoding side one nice property of these filters is that each operation is entirely reversible

PNG takes an uncompressed RGB image and then filters each channel of the image row wise picking a filter through heuristics



## LZSS

huffman codes treat values independently ,but in the real images redundancy also happens across sequences of pixels

### Run Length Encoding  RLE

take the advantage of repeative neighboring pixels 

the basic premise of RLE involves compressing a sequence of similar pixel values as the pixel value and the number of continuous occurrences of that value

In images where a lot of the pixels along the row have the same color ,RLE can be quite effective
![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/undefinedPasted%20image%2020230706223321.png)

But ,in this image,RLE doesn't really perform well
![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/undefinedPasted%20image%2020230706223350.png)

because the redundancy happens across a sequence of 2 pixels

### LZSS

a class of algorithms that are better tailored to handing repetitions of sequences are Lempel-Ziv Schemes LZ

the basic idea of LZSS involves back referencing
![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/undefinedPasted%20image%2020230706224220.png)

save some space by referencing that earlier sequence instead of outputting the same sequence of characters

- Sliding Window
- Look-Ahead Buffer
- Seach Buffer 

both of which are given a specific size

 ![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/undefinedPasted%20image%2020230706225725.png)

the compressor looks for the largest string in the  lookahead buffer that matches text in the current sliding window

the big idea LZSS is we can encode this character as an offset and a length

![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/undefinedPasted%20image%2020230706230536.png)

a final encoding is the following sequence of characters and offset length pairs


generally define a sliding window length of 32KB ,also define a look ahead buffer of 258B

![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/undefinedPasted%20image%2020230706231304.png)


a final caveat of LZSS is that practice the algorithm only emits a back reference of length three or more

![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/undefinedPasted%20image%2020230707090127.png)





## Huffman coding


generate a set of codes where fewer bits are allocated to more frequent data is huffman codes

key idea here is the most frequently used pixels are higher up in the tree ,leading to a small bit representation

To get back the original image,a huffman decoder has to know the codes we used ,we need to store version of this tree whick makes huffman coding generally wastefull for smaller images,but for larger sets of data huffman can be a good choice.


## ShortComing

slowest  compression methods
![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/undefinedPasted%20image%2020230707094317.png)


next :QOI

