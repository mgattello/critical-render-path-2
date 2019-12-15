# Critical Render Path 2

In the Critical Render Path 1 (https://github.com/mgattello/critical-render-path), we saw what it is, how to optimize the first two phases. In this chapter, we'll see how to optimize the script.

![crp](/img/d.png)

## Optimize script (C)

Javascript can both access and edit the DOM and the CSSOM. For example, once the script tag in the HTML is discovered, the DOM is paused and a call to the server is sent. After the execution of the script, it's time for the CSSOM to be created. That why we call Javascript a `parser blocking`.

### Use script async

What happens when using `<script>`?

The HTML is parsing, stops to download execute the script and only after finish the process.

Using `<script async>` allows you to avoid pausing the DOM while you download the JS file. After that, it will stop the HTML parsing only when the script is executed. The Google Analytics script could be loaded asynchronously for example.

When to use it?

- When you need to load third-party code that doesn't refer to other parts of the website.
- When the loading order of this script doesn't affect the execution of others scripts.

### Use script defer

It will not influence the loading of the page but will be executed only after the HTML is been parsed and in order of appearance.

When to use it?

- For scripts that will act on the DOM but also not critical

### Conclusion 

- Use `<script>` for your application (critical) scripts
- Use `<script async>` for third party scripts that not affect the DOM
- Use `<script defer>` for third party scripts that are not important and can be executed after the page is loaded

## Minimize DOM manipulation

If you open the project and open the Dev Tools in your browser, you will see that someone is saying Hello!. First, let's test the `<script async>`.
This is our starting point:

```
    <script src="script.js"></script>
    <script src="./script2.js"></script>
    <script src="./script3.js"></script>
```

This is the command line:

```
Hello from script 1
Hello from script 2
Hello from script 3
DOM loaded and parsed
All resources loaded

```
Let's make the script2 async:

```
  <script async src="./script2.js"></script>
```
Now let's check the command line:

```
Hello from script 1
Hello from script 3
DOM loaded and parsed
Hello from script 2
All resources loaded
```

As you can see using async not only allows you to avoid the DOM to be loaded but it's a perfect feature for minimizing the manipulation of the DOM.

## Render Tree (D)

After the DOM, the CSSOM and the execution of JS, finally we have the Render Tree, that sum all the elements gather until now and create the structure.

## Layout (E)

Phase in which layout and padding are rendered.

## Paint (F)

Pixel are coloured.

## JS Redraw (G)

Redraw JS file when needed, but we can't rely on it because is not efficient. 