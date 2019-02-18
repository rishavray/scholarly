# scholarly
This is the fork of [scholarly](https://github.com/OrganicIrradiation/scholarly) which is a module that allows you to retrieve author and publication information from [Google Scholar](https://scholar.google.com) in a friendly, Pythonic way. In this fork, the bibtex entries are also retrieved along with other information in the Publication object.


## Usage
Because `scholarly` does not use an official API, no key is required. Simply:

```python
import scholarly

print(next(scholarly.search_author('Steven A. Cholewiak')))
```

### Methods
* `search_author` -- Search for an author by name and return a generator of Author objects.

```python
    >>> search_query = scholarly.search_author('Marty Banks, Berkeley')
    >>> print(next(search_query))
    {'_filled': False,
     'affiliation': 'Professor of Vision Science, UC Berkeley',
     'citedby': 17758,
     'email': '@berkeley.edu',
     'id': 'Smr99uEAAAAJ',
     'interests': ['vision science', 'psychology', 'human factors', 'neuroscience'],
     'name': 'Martin Banks',
     'url_picture': 'https://scholar.google.com/citations?view_op=medium_photo&user=Smr99uEAAAAJ'}
```

* `search_keyword` -- Search by keyword and return a generator of Author objects.

```python
    >>> search_query = scholarly.search_keyword('Haptics')
    >>> print(next(search_query))
    {'_filled': False,
     'affiliation': 'Stanford University',
     'citedby': 31731,
     'email': '@cs.stanford.edu',
     'id': '4arkOLcAAAAJ',
     'interests': ['Robotics', 'Haptics', 'Human Motion Understanding'],
     'name': 'Oussama Khatib',
     'url_picture': 'https://scholar.google.com/citations?view_op=medium_photo&user=4arkOLcAAAAJ'}
```

* `search_pubs_query` -- Search for articles/publications and return generator of Publication objects. The bibref attribute contains the Bibtex entry for the particular publication.

```python
    >>> search_query = scholarly.search_pubs_query('Perception of physical stability and center of mass of 3D objects')
    >>> print(next(search_query))
    {'_filled': False,
     'bib': {'abstract': 'Humans can judge from vision alone whether an object is '
                         'physically stable or not. Such judgments allow observers '
                         'to predict the physical behavior of objects, and hence '
                         'to guide their motor actions. We investigated the visual '
                         'estimation of physical stability of 3-D objects (shown '
                         'in stereoscopically viewed rendered scenes) and how it '
                         'relates to visual estimates of their center of mass '
                         '(COM). In Experiment 1, observers viewed an object near '
                         'the edge of a table and adjusted its tilt to the '
                         'perceived critical angle, ie, the tilt angle at which '
                         'the object …',
             'author': 'SA Cholewiak and RW Fleming and M Singh',
             'eprint': 'http://jov.arvojournals.org/article.aspx?articleid=2213254',
             'title': 'Perception of physical stability and center of mass of 3-D '
                      'objects',
             'url': 'http://jov.arvojournals.org/article.aspx?articleid=2213254'},
     'bibref': u'@article{cholewiak2015perception,\n  title={Perception of physical stability and center of mass of 3-D objects},\n  author={Cholewiak, Steven A and Fleming, Roland W and Singh, Manish},\n  journal={Journal of vision},\n  volume={15},\n  number={2},\n  pages={13--13},\n  year={2015},\n  publisher={The Association for Research in Vision and Ophthalmology}\n}\n',
     'citedby': 14,
     'id_scholarcitedby': '15736880631888070187',
     'source': 'scholar',
     'url_scholarbib': 'https://scholar.googleusercontent.com/scholar.bib?q=info:K8ZpoI6hZNoJ:scholar.google.com/&output=citation&scisig=AAGBfm0AAAAAXGSbUf67ybEFA3NEyJzRusXRbR441api&scisf=4&ct=citation&cd=0&hl=en'}

```


### Example
Here's a quick example demonstrating how to retrieve an author's profile then retrieve the titles of the papers that cite his most popular (cited) paper.

```python
    >>> # Retrieve the author's data, fill-in, and print
    >>> search_query = scholarly.search_author('Steven A Cholewiak')
    >>> author = next(search_query).fill()
    >>> print(author)

    >>> # Print the titles of the author's publications
    >>> print([pub.bib['title'] for pub in author.publications])

    >>> # Take a closer look at the first publication
    >>> pub = author.publications[0].fill()
    >>> print(pub)

    >>> # Which papers cited that publication?
    >>> print([citation.bib['title'] for citation in pub.get_citedby()])
```


## Installation
clone the package using git:

    git clone https://github.com/rishavray/scholarly.git


## Requirements
Requires [arrow](http://crsmithdev.com/arrow/), [Beautiful Soup](https://pypi.python.org/pypi/beautifulsoup4/), [bibtexparser](https://pypi.python.org/pypi/bibtexparser/), and [requests[security]](https://pypi.python.org/pypi/requests/).


## License
The original code that this project was forked from was released by [Bello Chalmers](https://github.com/lbello/chalmers-web) under a [WTFPL](http://www.wtfpl.net/) license. In keeping with this mentality, all code is released under the [Unlicense](http://unlicense.org/).
