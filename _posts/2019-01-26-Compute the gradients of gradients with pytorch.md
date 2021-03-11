---
title: 'Computing the gradients of gradients with pytorch'
date: 2019-01-26
permalink: /posts/2019/01/compute-gradients-of-gradients/
tags:
  - Reinforcement Learning
  - Pytorch
---

## Computing the gradients of gradients with pytorch

Rencently, I am working with GAN and RL.For the puspose of smoothing the learning process, I need compute the gradients of gradients(the second order of gradients).

If you are fimilar with WGAN, you must know the lipschitz constrain. Implementation with GAN-gp, you need compute the gradients of D as constrain.

That is easy for you if you work with Tensorflow (I hate this tool!).

Pytorch is a useful tool, I love it.

We can use torch.autograd.grad to implement the code.

You can click [here](https://pytorch.org/docs/0.4.1/autograd.html#) to read the docs.

Let's read the core code.

```
torch.autograd.grad(outputs, inputs, grad_outputs=None, retain_graph=None, create_graph=False, only_inputs=True, allow_unused=False)[source]
Computes and returns the sum of gradients of outputs w.r.t. the inputs.

grad_outputs should be a sequence of length matching output containing the pre-computed gradients w.r.t. each of the outputs. If an output doesn’t require_grad, then the gradient can be None).

If only_inputs is True, the function will only return a list of gradients w.r.t the specified inputs. If it’s False, then gradient w.r.t. all remaining leaves will still be computed, and will be accumulated into their .grad attribute.

Parameters:	
outputs (sequence of Tensor) – outputs of the differentiated function.
inputs (sequence of Tensor) – Inputs w.r.t. which the gradient will be returned (and not accumulated into .grad).
grad_outputs (sequence of Tensor) – Gradients w.r.t. each output. None values can be specified for scalar Tensors or ones that don’t require grad. If a None value would be acceptable for all grad_tensors, then this argument is optional. Default: None.
retain_graph (bool, optional) – If False, the graph used to compute the grad will be freed. Note that in nearly all cases setting this option to True is not needed and often can be worked around in a much more efficient way. Defaults to the value of create_graph.
create_graph (bool, optional) – If True, graph of the derivative will be constructed, allowing to compute higher order derivative products. Default: False.
allow_unused (bool, optional) – If False, specifying inputs that were not used when computing outputs (and therefore their grad is always zero) is an error. Defaults to False.
```

That's ok. Here I give you guys two example.

## Example 1: GAN-gp :
```
def compute_gradient_penalty(D, real_samples, fake_samples):
    """Calculates the gradient penalty loss for WGAN GP"""
    # Random weight term for interpolation between real and fake samples
    alpha = Tensor(np.random.random((real_samples.size(0), 1, 1, 1)))
    # Get random interpolation between real and fake samples
    interpolates = (alpha * real_samples + ((1 - alpha) * fake_samples)).requires_grad_(True)
    d_interpolates = D(interpolates)
    fake = Variable(Tensor(real_samples.shape[0], 1).fill_(1.0), requires_grad=False)
    # Get gradient w.r.t. interpolates
    gradients = autograd.grad(
        outputs=d_interpolates,
        inputs=interpolates,
        grad_outputs=fake,
        create_graph=True,
        retain_graph=True,
        only_inputs=True,
    )[0]
    gradients = gradients.view(gradients.size(0), -1)
    gradient_penalty = ((gradients.norm(2, dim=1) - 1) ** 2).mean()
    return gradient_penalty
```

If you are interested in WGAN-gp, you can click [here](https://github.com/sweetice/GAN-course-note/blob/master/GAN-code/wgan-gp.py) to read the core code. 


## Example 2: In any network :
```
    def compute_gradient_penalty(self, network, input):
        # ref: https://github.com/eriklindernoren/PyTorch-GAN/blob/master/implementations/wgan_gp/wgan_gp.py
        '''

        :param input: state[index]
        :param network: actor or critic
        :return: gradient penalty
        '''
        input_ = torch.tensor(input).requires_grad_(True)
        output = network(input_)
        musk = torch.ones_like(output)
        gradients = grad(output, input_, grad_outputs=musk,
                         retain_graph=True, create_graph=True,
                         allow_unused=True)[0]  # get tensor from tuple
        gradients = gradients.view(-1, 1)
        gradient_penalty = ((gradients.norm(2, dim=1) - 1) ** 2).mean()
        return gradient_penalty
```

If you are interested in reinforcement learning and general aritificial intelligence, you can click [here](https://github.com/sweetice/Deep-reinforcement-learning-with-pytorch). :)


Enjoy yourself :)
