# Siamese Network

original paper doesn't converge well, no discussion on dropout or batch norm on original Koch et.al paper.
loss doesn't drop. using abs() of two output of siameye 4096 each when substracted. and fed into sigmoid with BCEloss.

running with Lecunn contrast loss. Not following 1) and 4) at this point, with At&T sample not working. using 2) and 3) but with koch network 
except the fc up to pure 4096 loss drop is better, margin for now at 2.0

using batch norm is crusical ended up using 4096 with few fc to 5 then use contrast loss , margin 2 ok, tried 1 and 5, not good. 
conv layers are with relu and batch norm and dropout 0.2, still keep fc only with relu, need try out if dropout in fc help (mod4)

*******************************************************
*******************************************************
08/08/18 what actually worked, siameye1-copy-mod7_omniglot-dataloader_ori_network.ipynb
*******************************************************
I ended up following the paper, all conv the same as before but removed dropout at end of each conv, kept also batch norm
one linear from 9216 to 4096, then sigmoin on for each of the two output, get F.L1loss within the forward to get delta of 4096, then take it 
to another linear from 4096 to 1.  out put is batchsize*1. 

give this to BCElogisloss (logit has sigmoid in it and supposedly more stable than BCEloss (with sigmoid outputted from network).

out put 1 from net work mean similar, 0 means dissimilar. to get the "dissimilarity" for print out substracted 1-sigmid(output).

data loader has the option to pick pair with 50% similarity dissimilarity flag_train ON. if off second image is picked randomly from sets
with size of classes in dataset.

In terms of data loader, for train use 50%/50%. for checking the results either plotting pairs with 50%/50%, or made sure the first pair is 
similar, then the rest to check.

PNG (omniglot) reading had issue with PIL, picked cv2, but only did tensor coversion (/255), no other transformation. Very surprised
the original paper without dropout worked out with output of of linear into 4096 -> 1 dimension (batch size*1 tensor size).

output of BCElogit loss decided to keep sum instead of average (mean), better to see the data in time also make sure not particular images are 
pulling down the loss individualy.

It is not clear to me for other methods whether the issue was contrast loss function, last layers of network or dropout?

accuracy should not be bad , guessing 80-85%. can also try the model.pth on at&t photos as well.
reference (5) useful.

the train set is 30 language that their alphabet mixed in together 30*(20-30) = ~900 classes (~900 alphabet)
test set is remaining 20 language with alphabet mixed in together 20*(20-30)=  ~600 classes (~600 alphabet)

each alphabet has 20 letter

********************************************************
********************************************************

siameye1-copy-mod7_omniglot-dataloader_ori_network-acc

added accuracy, updated the dataloader to work on laptop and also get the actual label of images.

Train accuracy:  95.4%
Test accuracy:  70.4%   much lower than paper!, seems is the consistent observation in other blogs/github

wonder in 100 epochs was not enough!
********************************************************

1)https://www.cs.cmu.edu/~rsalakhu/papers/oneshot1.pdf
2)http://yann.lecun.com/exdb/publis/pdf/hadsell-chopra-lecun-06.pdf
3)https://hackernoon.com/facial-similarity-with-siamese-networks-in-pytorch-9642aa9db2f7
4)https://sorenbouma.github.io/blog/oneshot/
5)https://www.cl.cam.ac.uk/research/dtg/attarchive/facesataglance.html     #At&t faces
6)https://github.com/Goldesel23/Siamese-Networks-for-One-Shot-Learning


