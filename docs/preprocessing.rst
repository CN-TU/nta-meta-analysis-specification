.. _preprocessing:

Preprocessing
=============

1. **performed_feature_selection**
(boolean) It states if a feature selection process is carried out in the paper to select the suitable set of features for the analysis. Example:

  .. code-block:: none

	"performed_feature_selection": "true"

2. **packet_analysis_oriented**
(boolean) It states if, after the preprocessing phase, the data to analyze is intended to be explored packet by packet (e.g., by methods that perform deep packet inspection). Example:

  .. code-block:: none

	"packet_analysis_oriented": "false"

3. **flow_analysis_oriented**
(boolean) It states if, after the preprocessing phase, the data to analyze is intended to be explored flow by flow. Example:

  .. code-block:: none

	"flow_analysis_oriented": "true"

4. **feature_aggregation_analysis_oriented**
(boolean) It states if, after the preprocessing phase, the data to analyze has been aggregated according to either features or flows. Therefore, the final analysis will not explore flows or packets, but usually networks as a whole by studying the aggregated values (e.g., time series showing the use of network resources). Example:

  .. code-block:: none

	"feature_aggregation_analysis_oriented": "false"

5. **tools**
(array of strings) Here we describe the tools used for the preprocessing (data extraction, feature generation and transformations). We use the following keys for the nomenclature: 
     
    a) if they are released tools, software or suites, they must be appear with the corresponding name; e.g., *thsark*, *silk*, *tcpdump*.  
    b) if they consist on scripts or plugins for well-known programming languages, suites, packages or environments, the name must reflect such dependency; e.g., *matlab_scripts*, *java_scripts*, *python_scripts*. 
    c) only the top-dependency must be shown (e.g., *matlab*). Additional *relevant* packages running under the same environment should be also added as *tools*.
    d) names start with *own_* if they are presented in the paper or referred to previous publications by the same authors (and they do not fit case 'a.'); e.g., *own_matlab_scripts*.
 
  Example:

  .. code-block:: none

	"tools": ["tshark", "own_python_scripts", "own_perl_scripts"]

6. *tool_details*
(array of strings) This field is linked to the previous field (*tool*) and expresses important details about the referred tools (e.g., version, release). Details shows a one-to-one correspondence with tools and respects the array order (i.e., *tool_details[1]* is for *tool[1]*). If no details are required, *none* should be written in the corresponding place. Example:

  .. code-block:: none

	"tool_details": ["v2.0.0", "none", "none"]

7. **availability_of_tools**
(array of strings) This field is linked to the *tool* field and expresses the availability of the referred tools. Availability shows a one-to-one correspondence with tools and respects the array order (i.e., *availability_of_tools[1]* is for *tool[1]*). Please, consider carefully the following default labels (values):

    *public*, *private*, *public_on_demand*, *commercial*

  Example:

  .. code-block:: none

	"availability_of_tools": ["public", "private", "private"]

8. **normalization_type**
(string) This field saves information about possible normalization of numerical data. *no* stands for cases where no normalization is applied but numerical attributes are used. *not_applicable* is for cases where normalization makes no sense (e.g., all analyzed fields are nominal or categories). Please, consider carefully the following default labels (values):

    *no*, *not_applicable*, *range*, *zscore*, *decimal_scaling*, *quartile*
      do not confuse *quartile* with *quantile*. *quartile* normalization uses *Q1* (25th percentile) and *Q3* (75th percentile) for normalization.

  Example:

  .. code-block:: none

	"normalization_type": "range"

9. **transformations**
(array of strings) This field collects all transformations that are performed after the dataset retrieval and previous to the analysis phase (i.e., they are part of the data preparation). Please, consider carefully the following listed operations (values):

    *no*, *sampling*, *filtering*, *log*, *map*, *graph*, *feature_aggregation*, *flow_extraction*, *entropy*, *time_series*, *feature_operation*, *class_separation*

  Example:

  .. code-block:: none

	"transformations": ["sampling", "flow_extraction", "class_separation"]

10. **final_data_format**
(string) It collects the format of data after the preprocessing and previous to the analysis phase. Please, consider carefully the following default labels (values):

    * *numerical_vectors*
    * *nominal_vectors*
    * *mixed_vectors*
    * *strings*
    * *time_series*

  Example:

  .. code-block:: none

	"final_data_format": "numerical_vectors"

11. *feature_selections*
(array of objects) *feature_selections* can contain several *feature_selection-objects*. A *feature_selection-object* is composed of several fields: 

  11.1. *name*
   (string) The name that identifies the feature selection technique. Example:

   .. code-block:: none
  
        "name": "forward_selection"

  11.2. *type*
   (string) It identifies the type of feature selection method. Please, consider carefully the following default labels (values): 

      * *wrapper*
      * *filter*
      * *hybrid*
      * *nest*
         when it embeds or operates in a higher level than other nested methods. 
      * *feature_reduction*
         when it refers to methods that change the space and transform the initial set of features into a new set of features with less dimensions (e.g., PCA, LDA). 

   Example:

   .. code-block:: none
  
        "type": "wrapper"

  11.3. *classifier*
   (string) It identifies the wrapped classifier that is used to evaluate the subset performance. If *classifier* is not applicable (e.g., for filters), write *none*. Example:

   .. code-block:: none
  
        "classifier": "naive_bayes"

  11.4. *role*
   (string) This field is meaningful when diverse feature selection methods are compared. Default values are: *main*, when the method led to the best solutions; and *competitor* for other cases. If only one feature selection method is used, it is always *main*. Example:

   .. code-block:: none
  
        "role": "main"

12. *packets*
(array of objects) *packets* can contain several *packet-objects*. A *packet-object* is defined when analysis in the paper are conducted on packets, i.e., analysis tools check packets independently or/and packet contents. A *packet-object* is composed of several fields: 

  12.1. *selection*
   (string) It identifies how the features extracted to analyze packets where selected. Please, consider carefully the following default labels (values):

    * *in_dataset* 
       if the analyzed feature set is exactly the same feature set of the dataset before preprocessing.
    * *feature_selection*
       if a feature selection process was conducted and led to the current feature subset. 
    * *study_based*
       if the selected features are taken from a previous study referred in the paper.
    * *tool_based*
       if the selected features are obtained from an extraction or preprocessing tool.
    * *expert_knowledge*
       if the selection of features is endorsed by reasoning and proper explanations in the paper.

   Example:

   .. code-block:: none
  
        "selection": "in_dataset"

  12.2. *role*
   (string) This field is meaningful when diverse preprocessing methods are compared. Default values are: *main*, when the method led to the best solutions; and *competitor* if not. If only one feature extraction method is used, it is always *main*. For the specific case of *packets*, packet inspection is usually used as baseline or ground truth for validating flow-based analysis. In such case, write *validation* in this field. Example:

   .. code-block:: none
  
        "role": "validation"

  12.3. *main_goal*
   (string) This field saves the main goal of preparing the data according to this packet-based format. Please, consider the following possible labels (values):

    *anomaly_detection*, *traffic_classification*, *botnet_detection*, *specific_malware_detection*, *network_properties_monitoring*, *dos_detection*, *ddos_detection*, *user_to_root_detection*, *probe_detection*, *p2p_traffic_classification*, *application_classification*, *remote_to_local_detection*, *attack_classification*, *p2p_botnet_detection*, *application_protocol_detection*, *classification_of_encrypted_traffic*, *traffic_rate_prediction*, *traffic_visualization*, *classification_for_qos*, *http_intrusion_detection*
   
   Example:

   .. code-block:: none
  
        "main_goal": "traffic_classification"

  12.4. *features*
   (see below)


13. *flows*
(array of objects) *flows* can contain several *flow-objects*. A *flow-object* is defined when analysis in the paper are conducted on flows, i.e., analysis tools check the behaviour of connection and connection attempts. A *flow-object* is composed of several fields: 

  13.1. *selection*
    Like in 12.1.

  13.2. *role*
    Like in 12.2.

  13.3. *main_goal*
    Like in 12.3. 

  13.4. *active_timeout*
   (numerical, in seconds) This field defines the maximum duration of a flow. Example:

   .. code-block:: none
  
        "active_timeout": 60

  13.5. *idle_timeout*
   (numerical, in seconds) This field defines the time in which, if no activity has been detected, the flow is considered as finished. Example:

   .. code-block:: none
  
        "idle_timeout": 5

  13.6. *bidirectional*
   (string) This field marks if transmissions between two devices A and B are considered monodirectional (*false*), i.e., A>B and A<B are two different flows; or bidirectional (*true*), i.e., A>B and A<B belong to the same flow . Example:

   .. code-block:: none
  
        "bidirectional": "true"

  13.7. *features*
   (see below)

  13.8. *key*
   (see below)

14. *feature_aggregations*
(array of objects) *feature_aggregation* can contain several *feature_aggregation-objects*. A *feature_aggregation-object* is defined when analysis in the paper are conducted on aggregation of features or flows, i.e., analysis tools usually describe networks as a whole. A *feature_aggregation-object* is composed of several fields: 

  14.1. *selection*
    Like in 12.1.

  14.2. *role*
    Like in 12.2.

  14.3. *main_goal*
    Like in 12.3. 

  14.4. *active_timeout*
    Like in 13.4.

  14.5. *features*
   (see below)

  14.6. *key*
   (see below)


*features*

*key*

JSON example (preprocessing, complete)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: none

  "preprocessing": {
    "performed_feature_selection": "true",
    "packet_analysis_oriented": "false",
    "flow_analysis_oriented": "true",
    "feature_aggregation_analysis_oriented": "false",
    "tools": ["tshark", "own_perl_scripts"],
    "tool_details": ["v2.0.0", "none"],
    "availability_of_tools": ["public", "private"],
    "normalization_type": "range",
    "transformations": ["flow_extraction","log","time_series", "feature_operation", "class_separation"],
    "final_data_format": "numerical_vectors",
    "feature_selections": [
        {
            "name": "max-relevance min-redundancy filter (correlation and MI based)",
            "type": "filter",
            "classifier": "none",
            "role": "main"
        }
    ],
    "flows": [
        {
            "selection": "expert_knowledge",
            "role": "main",
            "main_goal": "traffic_classification",
            "active_timeout": 60,
            "idle_timeout": 60,
            "bidirectional": "false",
            "features": [
                {"log": ["octetTotalCount"]},
                {"log": ["packetTotalCount"]},
                "_activeForSeconds",
                {"log": [{"divide": ["octetTotalCount", "_activeForSeconds"]}]},
                {"log": [{"divide": ["packetTotalCount", "_activeForSeconds"]}]},
                "__maximumConsecutiveSeconds",
                "__minimumConsecutiveSeconds",
                {"maximum": ["_interPacketTimeMicroseconds"]},
                {"minimum": ["_interPacketTimeMicroseconds"]},
                "__numberof_activity_intervals",
            ],
            "key": [
                "sourceIPv4Address", 
                "destinationIPv4Address",
                "protocolIdentifier"
            ]
        },
        {
            "selection": "feature_selection",
            "role": "main",
            "main_goal": "traffic_classification",
            "active_timeout": 60,
            "idle_timeout": 60,
            "bidirectional": "false",
            "features": [
                {"log": ["octetTotalCount"]},
                {"log": [{"divide": ["octetTotalCount", "_activeForSeconds"]}]},
                {"maximum": ["_interPacketTimeMicroseconds"]},
                {"minimum": ["_interPacketTimeMicroseconds"]},
            ],
            "key": [
                "sourceIPv4Address", 
                "destinationIPv4Address",
                "protocolIdentifier"
            ]
        }
    ]
  },
