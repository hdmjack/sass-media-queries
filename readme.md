# CSS and Media Query Strategies Using SASS

Inspired by the [less-media-queries](https://github.com/buymeasoda/less-media-queries) project, the goal here is to use SASS to deliver responsive and retina-ready CSS, while providing a backup stylesheet for unsupported browsers (IE8 and earlier).   

##Caveat

At this time SASS does not support anything like LESS' buffering functionality, which is essential for providing an elegant fallback stylesheet solution. As such, the code here makes use of Stan Angeloff's very spiffy @buffer branch [explained here](https://github.com/nex3/sass/issues/116#issuecomment-7355117).

As the @buffer feature is __not yet (and may never be) in SASS core__, SASS Media Queries should __only be treated as proof-of-concept__ until @buffer, or similar functionality, is fully supported.

##Differences from LESS Media Queries

There are a few important differences in structure to make note of if you are already familiar with LESS Media Queries, necessitate by the different buffering implementation.

* The buffers are directly referenced in modern.scss and legacy.scss, instead of being wrapped in classes

* Referecing empty buffers is not currently supported, so buffers should be commented out or deleted from the modern.scss and legacy.scss if they are unused

#Sample Component CSS

CSS rules for a single component can be grouped together, along with the design and layout adjustments for different media query breakpoints and resolutions.

	.selector {
		/* Universal rules for #selector go here, all browsers get these rules */
	}

	@buffer up320 {
		.selector {
			/* Rules for any desired media query triggers go here, in this case it's a min-width: 320px rule */
		}
	}

	@buffer up768 {
		.selector a {
			/* Additional rules. Not all defined trigger points need to be included, only where relevant */
		}
	}

	@buffer allx2 {
		.selector {
			/* Rules for image overrides that apply at all dimensions for user agents that meet the @2x / hi-dpi rules */
		}
	}

	@buffer up768x2  {
		.selector {
			/* Hi-dpi / @2x rules for the 768px breakpoint */
		}
	}

# Sample SASS Stucture

The output files are generated from the same source CSS, so there is no double maintenance or divergence of styles.

* `_utilities.scss` contains the media query trigger points, named media query variables and pre-defined output for legacy and modern styles
* `_styles.scss` is a demo file that represents your project styles. In practise, this would be a collection of project structured project files

## Legacy SASS

	@import "utilities";
	@import "styles";

	@flush up320;
	@flush up768;

## Modern SASS

	@import "utilities";
	@import "styles";

    @media #{$up320} {
        @flush up320;
    }

    @media #{$up768} {
        @flush up768;
    }

    @media #{$x2} {
        @flush allx2;
    }

    @media #{$x2}, #{$up768} {
        @flush up768x2;
    }

# Sample CSS Output

Based on the example "sample component" code above, this is the resulting output.

## Legacy CSS

	.selector {
		/* Universal rules for #selector go here, all browsers get these rules */
	}
	.selector {
		/* Rules for any desired media query triggers go here, in this case it's a min-width: 320px rule */
	}
	.selector a {
		/* Additional rules. Not all defined trigger points need to be included, only where relevant */
	}

## Modern CSS

	.selector {
		/* Universal rules for #selector go here, all browsers get these rules */
	}

	@media only screen and (max-width: 320px) {
		.selector {
			/* Rules for any desired media query triggers go here, in this case it's a min-width: 320px rule */
		}
	}

	@media only screen and (min-width: 768px) {
		.selector a {
			/* Additional rules. Not all defined trigger points need to be included, only where relevant */
		}
	}

	@media only screen and (min-device-pixel-ratio: 1.3), only screen and (-webkit-min-device-pixel-ratio: 1.3), only screen and (-o-min-device-pixel-ratio: 13/10), only screen and (min-resolution: 120dpi) {
		.selector {
			/* Rules for image overrides that apply at all dimensions for user agents that meet the @2x / hi-dpi rules */
		}
	}

	@media only screen and (min-device-pixel-ratio: 1.3), only screen and (-webkit-min-device-pixel-ratio: 1.3), only screen and (-o-min-device-pixel-ratio: 13/10), only screen and (min-resolution: 120dpi), only screen and (min-width: 768px) {
		.selector {
			/* Hi-dpi / @2x rules for the 768px breakpoint */
		}
	}

Many thanks to [buymeasoda](https://github.com/buymeasoda) for letting me heavily borrow documentation.