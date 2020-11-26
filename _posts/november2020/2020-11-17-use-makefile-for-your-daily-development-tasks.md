---
layout: post
title: Use Makefile for your daily development tasks
date: 2020-11-17
related_image: /assets/images/2020-11-17.jpg
---
<div class="view overlay">
	<img class="card-img-top" src="{{ page.related_image }}" alt="Card image cap">
    <a href="#!">
        <div class="mask rgba-white-slight"></div>
    </a>
</div>

A typical routine of a developer is code, build, unit test, and deploy. Most of the time, developers do all these tasks in their local first and once everything is fine then they move to the next state. This will be a very much repeated process when we work on a microservices architecture. 

From my experience, If I can list out a typical list of tasks for a go language-based microservice. Those are

1. Clean
2. Format
3. Lint
4. Dependencies
5. Run Unit tests
6. Build
7. Push to Dev Quay
8. Coverage Report

All these steps can be placed in a simple Makefile and then onwards, the same can be used by anyone to quickly take the charge and move on. This kind is one of the best practices when it comes to day-to-day development activity [I am not denying that there are multiple ways of achieving the same]. Here is the typical Makefile from my experience.

The below sample make file is specific to Go language-based environment but I believe the idea can be used to leverage with any other project. 

<!-- HTML generated using hilite.me --><div style="background: #ffffff; overflow:auto;width:auto;border:solid gray;border-width:.1em .1em .1em .8em;padding:.2em .6em;"><table><tr><td><pre style="margin: 0; line-height: 125%"> 1
 2
 3
 4
 5
 6
 7
 8
 9
10
11
12
13
14
15
16
17
18
19
20
21
22
23
24
25
26
27
28
29
30
31
32
33
34
35
36
37
38
39
40
41
42
43
44
45
46
47
48
49</pre></td><td><pre style="margin: 0; line-height: 125%"><span style="color: #008800; font-style: italic"># Enable dependencies and configure the private repo</span>
GO = GO111MODULE=on GOPRIVATE=&lt;your private repo&gt;.cisco.com
EXEFILE = ./bin/&lt;exe file name&gt;
MAIN_GO = &lt;your service main.go file location&gt;

<span style="color: #008800; font-style: italic"># help messages</span>
help:
    @echo <span style="color: #0000FF">&#39;Available commands for the &lt;your micro service name&gt;&#39;</span>
    @echo <span style="color: #0000FF">&#39;Usage&#39;</span>
    @echo <span style="color: #0000FF">&#39;  all - Run all the available commands&#39;</span>
    @echo <span style="color: #0000FF">&#39;  clean - Run the clean up&#39;</span>
    @echo <span style="color: #0000FF">&#39;  setup -  Run the initial setup instructions&#39;</span>
    @echo <span style="color: #0000FF">&#39;  format - Run code format&#39;</span>
    @echo <span style="color: #0000FF">&#39;  staticanalysis - Run the static code analysis&#39;</span>
    @echo <span style="color: #0000FF">&#39;  dependencies - Run the dependencies&#39;</span>
    @echo <span style="color: #0000FF">&#39;  unittest - Run unit tests&#39;</span>
    @echo <span style="color: #0000FF">&#39;  localbuild - Build the executable&#39;</span>

<span style="color: #008800; font-style: italic"># Run all the available commands in the below order</span>
all: <span style="color: #0000FF">clean setup dependencies format staticanalysis unittest localbuild</span>

clean:
    @echo <span style="color: #0000FF">&quot;Cleaning the previous executable and coverage reports&quot;</span>
    rm -f coveragereport.out report.json
    rm -f <span style="color: #000080; font-weight: bold">${</span>EXEFILE <span style="color: #000080; font-weight: bold">}</span>

setup:
    @echo <span style="color: #0000FF">&quot;Add any steps that are needed&quot;</span>
    &lt;some installation tasks&gt;
format:
    @echo <span style="color: #0000FF">&quot;Code Formatting&quot;</span>
    <span style="color: #000080; font-weight: bold">${</span>GO<span style="color: #000080; font-weight: bold">}</span> fmt ./...
staticanalysis:
    @echo <span style="color: #0000FF">&quot;&quot;</span>
    golangci-lint run -c .golangci.yaml
dependencies:
    @echo <span style="color: #0000FF">&#39; Load Dependencies&#39;</span>
    <span style="color: #000080; font-weight: bold">${</span>GO<span style="color: #000080; font-weight: bold">}</span> mod tidy
    <span style="color: #000080; font-weight: bold">${</span>GO<span style="color: #000080; font-weight: bold">}</span> mod download
unittest:
    @echo <span style="color: #0000FF">&quot;Executing Unit tests&quot;</span>
    <span style="color: #000080; font-weight: bold">${</span>GO<span style="color: #000080; font-weight: bold">}</span> test -v covermode=atomic -count=1 ./... -coverprofile coveragereport.out
    <span style="color: #000080; font-weight: bold">${</span>GO<span style="color: #000080; font-weight: bold">}</span> test -race test -covermode=atomic -count=1 ./... -json &gt; report.json
localbuild:
    @echo <span style="color: #0000FF">&quot;Loca build&quot;</span>
    <span style="color: #000080; font-weight: bold">${</span>GO<span style="color: #000080; font-weight: bold">}</span> build -o <span style="color: #000080; font-weight: bold">${</span>EXEFILE<span style="color: #000080; font-weight: bold">}</span> <span style="color: #000080; font-weight: bold">${</span>MAIN_GO<span style="color: #000080; font-weight: bold">}</span>
    docker build -t &lt;your tag name&gt; .
    docker tag &lt;your tag name&gt; &lt;your quay repo location&gt;:latest
    docker push &lt;your quay repo location&gt;:latest
</pre></td></tr></table></div>

