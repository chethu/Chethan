---
title: Spannable Text
tags: Android
article_header:
  type: cover
  image:
---



#### Spannable Text
Spans are helpful to style text, it help us to change the color of specific portion of the text, help us to make specific portion to be clickable, resizable , there many other use cases where we make use of the spannable text. Frequently, you come across one common style where you want to change the color of some portion and make that portion to be clickable. 

Below is simple demonstration of how you can organise your code for spannable text . In this snapshot, The text terms&contion , Privacy Policy is taking color and clickable. 

<div class="card">
  <div class="card__image">
     <img class="image" src="{{site.baseurl}}/assets/post/Spannable_text.gif"/>
  </div>
</div>

Clicking Text of Terms And Condition takes to another fragment to present detailed view of terms and condition, Similarly Privacy plolicy.

Below is code snap shot shows how you construct binding adapter for the same : 

```kotlin
object BindingAdapters {

    @JvmStatic
    @BindingAdapter("setTermsConditionText")
    fun TextView.setTermsConditionText(inputString: String?) {
        val titleText = resources.getText(R.string.termsConditionAgreementMessage) as SpannedString
        val annotations = titleText.getSpans(0, titleText.length, Annotation::class.java)
        val spannableString = SpannableString(titleText)
        val clickableSpanForTermsAndConditions = object : ClickableSpan() {
            override fun onClick(widget: View?) {
                findNavController().navigate(
                    HomeFragmentDirections.showTermsConditionFragment()
                )
            }

            override fun updateDrawState(ds: TextPaint?) {
                ds?.isUnderlineText = false
            }
        }

        val clickableSpanForPrivacyPolicy = object : ClickableSpan() {
            override fun onClick(widget: View?) {
                findNavController().navigate(
                    HomeFragmentDirections.showPrivacyFragment()
                )
            }

            override fun updateDrawState(ds: TextPaint?) {
                ds?.isUnderlineText = false
            }
        }

        annotations.forEach {
            when (it.value) {
                "terms_condition_link" -> {
                    setSpannableText(spannableString, clickableSpanForTermsAndConditions, titleText, it)
                }
                "privacy_link" -> {
                    setSpannableText(spannableString, clickableSpanForPrivacyPolicy, titleText, it)
                }
            }
        }



        this.apply {
            text = spannableString
            movementMethod = LinkMovementMethod.getInstance()
        }
    }

    private fun TextView.setSpannableText(
        spannableString: SpannableString, clickableSpanForTC: ClickableSpan, titleText: SpannedString, it: Annotation?
    ) {
        spannableString.apply {
            setSpan(
                clickableSpanForTC,
                titleText.getSpanStart(it),
                titleText.getSpanEnd(it),
                Spanned.SPAN_EXCLUSIVE_EXCLUSIVE
            )
            setSpan(
                ForegroundColorSpan(ContextCompat.getColor(context, R.color.colorAccent)),
                titleText.getSpanStart(it),
                titleText.getSpanEnd(it),
                0
            )
        }
    }

}
```

Calling binding adapter method in your XML : 

```kotlin
      <TextView
	android:layout_width="wrap_content"
	android:layout_height="wrap_content"
	app:setTermsConditionText="@{@string/termsConditionAgreementMessage}"
	app:layout_constraintBottom_toBottomOf="parent"
	app:layout_constraintLeft_toLeftOf="parent"
	app:layout_constraintRight_toRightOf="parent"
	app:layout_constraintTop_toTopOf="parent"/>
```

You can find project link here - 
https://github.com/chethu/Android-Spannable-Text
<!--more-->

