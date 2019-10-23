====================================================
Prohibit Setting Incompatible Source Type Properties
====================================================

Virtual DataPort version 5.0 allows setting the type of a field of a
view to a type T and in the “Source type properties” of the field,
select a subtype that is incompatible. Starting with Denodo 5.5, Virtual
DataPort no longer allows this. I.e. having a field of type ``text``
with subtype ``INTEGER`` is no longer valid.

Views with fields that have an incompatible subtype can be imported into
Virtual DataPort |version|. However, when you edit these views, the wizard
will not allow you to save the changes unless you assign a compatible
subtype to the field.
