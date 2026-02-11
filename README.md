# PDF Estimation of a Transformed Random Variable Using GAN


## Dataset Description
I used the **Indian Air Quality Dataset**, from which I extracted the **NO₂ (Nitrogen Dioxide)** concentration values.  
All missing values were removed, and the data was converted into floating-point format before further processing.

---

## Step 1: Transformation of the Random Variable

Each value of the original random variable \( x \) is transformed into \( z \) using the following transformation:

z = Tr(x) = x + a_r*sin(b_r *x) 

### Transformation Parameters


ar = 0.5*(rno%7)
br = 0.3*((rno%5) + 1)

The computed values are:
-  a_r = 2.0 
-  b_r = 0.3 

The transformed variable \( z \) is treated as a sample from an **unknown probability distribution**.

---

## Step 2: PDF Estimation Using GAN

## GAN Architecture

### Generator
- Input: Random noise sampled from \( \mathcal{N}(0,1) \)
- Architecture:
  - Linear(1 → 16) with ReLU
  - Linear(16 → 16) with ReLU
  - Linear(16 → 1)
- Output: Generated samples \( z_f \)

### Discriminator
- Input: Real or generated samples
- Architecture:
  - Linear(1 → 16) with ReLU
  - Linear(16 → 16) with ReLU
  - Linear(16 → 1) with Sigmoid
- Output: Probability that the sample is real

---

## Training Details

- Loss Function: Binary Cross-Entropy (BCE)
- Optimizer: Adam
- Learning Rate: 0.001
- Batch Size: 64
- Number of Epochs: 3000

### Training Procedure
-  trained the discriminator to distinguish between:
  - Real samples  z 
  - Fake samples  z_f = G(noise) 
- trained the generator to fool the discriminator into classifying generated samples as real.

---

## Step 3: PDF Approximation from Generator Samples

After training the GAN:
1. generated a large number of samples using the trained generator.
2. estimated the probability density using **histogram-based density estimation**.
3. compared the estimated PDF of generated samples with the histogram of real transformed samples.

---

## Results and Observations

### PDF Approximation
The generated samples closely match the distribution of real transformed data. The overall shape and spread of the distribution are well captured.

### Mode Coverage
The generator successfully captures the major modes of the real distribution. Minor deviations are observed in the tail regions.

### Training Stability
The discriminator and generator losses remain stable throughout training, and no mode collapse was observed.

### Quality of Generated Samples
The generated samples follow the same numerical range and statistical behavior as the real samples without explicitly modeling the PDF.


---

## Conclusion
Through this assignment, I demonstrated that a GAN can effectively learn an **unknown probability density function** using only samples from the transformed data. This approach avoids analytical assumptions and is suitable for modeling complex real-world distributions.

