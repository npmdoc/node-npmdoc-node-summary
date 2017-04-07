# api documentation for  [node-summary (v1.1.0)](https://github.com/jbrooksuk/node-summary)  [![npm package](https://img.shields.io/npm/v/npmdoc-node-summary.svg?style=flat-square)](https://www.npmjs.org/package/npmdoc-node-summary) [![travis-ci.org build-status](https://api.travis-ci.org/npmdoc/node-npmdoc-node-summary.svg)](https://travis-ci.org/npmdoc/node-npmdoc-node-summary)
#### Summarizes text using a naive summarization algorithm

[![NPM](https://nodei.co/npm/node-summary.png?downloads=true)](https://www.npmjs.com/package/node-summary)

[![apidoc](https://npmdoc.github.io/node-npmdoc-node-summary/build/screenCapture.buildNpmdoc.browser._2Fhome_2Ftravis_2Fbuild_2Fnpmdoc_2Fnode-npmdoc-node-summary_2Ftmp_2Fbuild_2Fapidoc.html.png)](https://npmdoc.github.io/node-npmdoc-node-summary/build/apidoc.html)

![npmPackageListing](https://npmdoc.github.io/node-npmdoc-node-summary/build/screenCapture.npmPackageListing.svg)

![npmPackageDependencyTree](https://npmdoc.github.io/node-npmdoc-node-summary/build/screenCapture.npmPackageDependencyTree.svg)



# package.json

```json

{
    "author": {
        "name": "James Brooks",
        "email": "jbrooksuk@me.com"
    },
    "bugs": {
        "url": "https://github.com/jbrooksuk/node-summary/issues"
    },
    "dependencies": {
        "lodash-node": "2.4.1",
        "sentence-tokenizer": "0.0.7"
    },
    "description": "Summarizes text using a naive summarization algorithm",
    "devDependencies": {
        "mocha": "1.15.1",
        "should": "2.1.1"
    },
    "directories": {},
    "dist": {
        "shasum": "c34857c465039d6b4b6df67ff63f02072dc1288c",
        "tarball": "https://registry.npmjs.org/node-summary/-/node-summary-1.1.0.tgz"
    },
    "gitHead": "91b677265430eb5b7e8f93c0a92592cfb40ccc6d",
    "homepage": "https://github.com/jbrooksuk/node-summary",
    "keywords": [
        "summary",
        "text",
        "summarization",
        "algorithm"
    ],
    "main": "./lib/summary.js",
    "maintainers": [
        {
            "name": "jbrooksuk",
            "email": "ukjbrooks@gmail.com"
        }
    ],
    "name": "node-summary",
    "optionalDependencies": {},
    "readme": "ERROR: No README data found!",
    "repository": {
        "type": "git",
        "url": "git://github.com/jbrooksuk/node-summary.git"
    },
    "scripts": {
        "test": "mocha --require should --reporter spec"
    },
    "version": "1.1.0"
}
```



# <a name="apidoc.tableOfContents"></a>[table of contents](#apidoc.tableOfContents)

#### [module node-summary](#apidoc.module.node-summary)
1.  [function <span class="apidocSignatureSpan">node-summary.</span>getSortedSentences (content, n, callback)](#apidoc.element.node-summary.getSortedSentences)
1.  [function <span class="apidocSignatureSpan">node-summary.</span>summarize (title, content, callback)](#apidoc.element.node-summary.summarize)



# <a name="apidoc.module.node-summary"></a>[module node-summary](#apidoc.module.node-summary)

#### <a name="apidoc.element.node-summary.getSortedSentences"></a>[function <span class="apidocSignatureSpan">node-summary.</span>getSortedSentences (content, n, callback)](#apidoc.element.node-summary.getSortedSentences)
- description and source-code
```javascript
getSortedSentences = function (content, n, callback) {
	if (typeof(n) === 'function') {
		callback = n;
		n = 0;
	}

	getSentencesRanks(content, function(dict) {
		getSortedSentences(content, dict, n, function(sorted_sentences) {
			if(sorted_sentences === '') {
				callback(new Error('Too short to summarize.'));
			} else {
				callback(null, sorted_sentences);
			}
		});
	});
}
```
- example usage
```shell
...
	if(err) {
		console.log("There was an error."); // Need better error reporting
	}

	console.log(summary);
});

summary.getSortedSentences(content, 5, function(err, sorted_sentences) {
    if(err) {
        console.log("There was an error."); // Need better error reporting
    }

    console.log(sorted_sentences);
});
...
```

#### <a name="apidoc.element.node-summary.summarize"></a>[function <span class="apidocSignatureSpan">node-summary.</span>summarize (title, content, callback)](#apidoc.element.node-summary.summarize)
- description and source-code
```javascript
summarize = function (title, content, callback) {
	var summary = [], paragraphs = [], sentence = '', err = false;
	getSentencesRanks(content, function(dict) {
		splitContentToParagraphs(content, function(paragraphs) {
			summary.push(title); // Store the title.

			_.each(paragraphs, function(p) {
				getBestSentence(p, dict, function(sentence) {
					if(sentence) summary.push(sentence);
				});
			});

			// If we only have a title, then there is an issue.
			if(sentence.length === 2) err = true;
			callback(err, summary.join("\n"));
		});
	});
}
```
- example usage
```shell
...
content += "The idea was born from our day-to-day need to be active on social media, look for the best content to share with our
 followers, grow them, and measure what content works best.\n";
content += "Who is on the team?\n";
content += "Ohad Frankfurt is the CEO, Shlomi Babluki is the CTO and Oz Katz does Product and Engineering, and I [Lior Degani] do
 Marketing. The four of us are the founders. Oz and I were in 8200 [an elite Israeli army unit] together. Emily Engelson does Community
 Management and Graphic Design.\n";
content += "If you use Percolate or read LinkedIn's recommended posts I think you'll love Swayy.\n";
content += "Want to try Swayy out without having to wait? Go to this secret URL and enter the promotion code thenextweb . The first
 300 people to use the code will get access.\n";
content += "Image credit: Thinkstock";

SummaryTool.summarize(title, content, function(err, summary) {
	if(err) console.log("Something went wrong man!");

	console.log(summary);

	console.log("Original Length " + (title.length + content.length));
	console.log("Summary Length " + summary.length);
	console.log("Summary Ratio: " + (100 - (100 * (summary.length / (title.length + content.length)))));
...
```



# misc
- this document was created with [utility2](https://github.com/kaizhu256/node-utility2)
