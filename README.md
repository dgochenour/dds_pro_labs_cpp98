# RTI Connext Professional Hands-On Exercises 
# (C++11 version)

This repo contains labs used in DDS QuickStart training, C++11 version. Before
any given lab can be compiled and run, rtiddsgen should be run on the \*.idl
file in each directory. When performing this step, be sure to set the following:
- Generation: Example Files = "<disable>"
- Generation: Type files = "update"
- Generation: Makefiles = "create"
- Language = "Modern C++ (C++ 11)"

## Lab 1. Out Of The Box

  - Create IDL, then generate code
  - Give the sample members some values, just so we aren't writing empty data
  - Speed up the writes on the publisher side by reducing the sleep from 1s to 500ms 
  - In the QoS XML:
    - update publication_name and subscription_name with meaningful values (student names, etc.)
    - replace the local schema URL with a remote one

## Lab 2. Loading a user-defined XML QoS profile

  - Rename `USER_QOS_PROFILES.xml` as `MY_QOS_PROFILES.xml`
    - Use Admin Console to confirm that the DDS entity names created in ex01 are no longer present-- that is becuase the newly-named file is not loaded by default like `USER_QOS_PROFILES.xml` was.
  - rename the library to "MyLibrary" and profile to "MyProfile"
  - remove `is_default_qos="true"` from MyProfile
  - change publisher and subscriber code to use custom qosProvider

## Lab 3. Replacing string literals in the source

  - We can declare const strings in the IDL so that string literals do not have to be manually entered in source. Let's do that for:
    - The Topic name
    - The QoS library and profile
  - The type support code now needs to be regenerated
    - Generation: Example Files = "<disable>"
    - Generation: Type files = "update"
    - Generation: Makefiles = "<disable>"
    - Language = "Modern C++ (C++ 11)"
  - Update `example_publisher.cxx` and `example_subscriber.cxx` to use these constants.

## Lab 4. Deadline Qos

  - Set deadline to 1.0 sec. on DataReader
  - Discuss why not working. (QoS mismatch)
  - Add Reader listener to note Qos Error
    - In C++98 documentation, search for DDS_QosPolicyId_t; here you can match "4" to "DDS_DEADLINE_QOS_POLICY_ID"
  - Fix Writer to offer Deadline of 500ms

## Lab 5. Reliability

  - remove `base_name="BuiltinQosLibExp::Generic.StrictReliable"` from QoS
  - Explicitly set reliability to RELIABLE_RELIABILITY_QOS in DatReader and DataWriter QoS.
  - Slow down writer to 1HZ and confirm that data is received at this rate.
  - Now set writer to wait for reliable acknowledgements.
  - Discuss writer write speed-- why did the data transfer slow down?
  - Speed up HB frequency in writer protocol to correct.

## Lab 6. Late Joiner History

  - Set Writer and Reader Durability to TRANSIENT_LOCAL_DURABILITY_QOS.
  - Set History to KEEP_LAST_HISTORY_QOS, depth 10 on the DataReader.
  - Set History to KEEP_LAST_HISTORY_QOS, depth 15 on the DataWriter.
  - Start Writer first, let it run for 10s or so, then start the Reader app and discuss late joiner results.

## Lab 7. Content Filtered Topic

  - Modify subscriber code to add a cft
    - add code to create the parameters and Filter
    - set the filter name
    - change the reader instantiation to use ContentFilteredTopic instead of normal topic
  - open admin console to show that no new topic is shown, the filtering is logical but
    does not spawn a new entity
