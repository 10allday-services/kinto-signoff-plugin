signoff: a kinto plugin
=======================

A Kinto plugin to ask for signoffs.


Configuration
-------------

You'll need to add `signoff` to the `kinto.includes` section in your
`config.ini` file::

    kinto.includes =
        signoff

At the moment, the signoff plugin will post back to an AWS stepfunction, so you
need to configure your AWS credentials::

    signoff.aws_access_key = <your AWS access key here>
    signoff.aws_secret_key = <your AWS secret key here>

It also makes use of the experimental `permissions` endpoint, so add the
following to the `config.ini` file::

    kinto.experimental_permissions_endpoint = True


Usage
-----

Ask for the signoffs
____________________

::
    POST /signoffs

      {
        "xpi": "<url of the XPI>",
        "checksum": "<sha256 of the XPI>",
        "message": "<some message to show to the reviewers>",
        "reviewers": ["reviewer1@example.com", "reviewer2@example.com", ...],
        "callback": {
          "activityARN": "<Amazon Resource Name of the activity to POST back to>"
        }
      }


Reviewer gets the list of reviews waiting on them
_________________________________________________

::
    GET /signoffs/for-me

    Returns:
    {
      "reviews": [
        {
          "signoff-id": "<signoff id>"
          "xpi": "<url of the XPI>",
          "message": "<some message to show to the reviewer>",
          "status": "<status of the review: UNANSWERED|REJECT|ACCEPT>"
        },
        {
          "signoff-id": "<signoff id>"
          "xpi": "<url of the XPI>",
          "message": "<some message to show to the reviewer>",
          "status": "<status of the review: UNANSWERED|REJECT|ACCEPT>"
        }
      ]
    }


Reviewer signs off on a review
______________________________

::
  POST /signoffs/{signoff-id}
  {
    "status": "<status of the signoff, either REJECT or ACCEPT>",
    "comment": "<comment from the reviewer>"
  }


Authors
-------

`signoff` was written by `Mathieu Agopian <mathieu@agopian.info>`_.
