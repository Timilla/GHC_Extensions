```mermaid
classDiagram
    class youtube_crawler {
        +search_youtube(query, max_num_video_ids=3)
    }
    class video_id_fetcher {
        +invert_dictionary(d)
        +get_egg_video_ids(count)
        +get_noun_ids_and_video_ids(num_videos_per_noun)
    }
    class video_fetcher {
        +fetch_video(url)
        +get_stream(video)
        +convert_video_to_mp4(filename)
        +converted_mp4_filename(filename)
        +downloaded_filename(url, title, extension)
        +sanitized_video_title(title)
        +get_video_id(url)
        +video_url(video_id)
    }
    class verify_lmdb {
        +main()
        +show_image(lmdb_value)
    }
    class train {
        +main()
    }
    class queries {
        <<static>> EGG_QUERIES
        <<static>> QUERIES_AND_NOUNS
    }
    class prepare_data {
        +create_lmdbs()
        +compute_image_mean()
    }
    class performance {
        +timeit(f)
    }
    class nms {
        +get_boxes(detection_output_file)
        +nms_detections(dets, overlap=0.3)
        +draw_boxes(dets)
    }
    class models {
      <<static>> MODELS
    }
    class main {
        +draw_noun_detections_on_video_frames(video_id, wnid)
        +test_detector_on_eggs()
    }
    class judge_predictions {
        +show_image(path)
        +print_text(text, img, screen)
        +finished()
        +add_line_to_log(path, code)
        +remove_line_from_log(path)
        +get_jpgs(dir)
    }
    class image_utils {
        +get_prepared_images(url, ms_between_frames, video_filename, wnid=None)
        +_prepare_images(video_filename, image_dir, ms_between_frames)
        +_prepare_image(bgr_image)
        +resize_and_paste(image, new_shape)
        +convert_bgr_to_rgb(image_filename, channel_order=(2,1,0))
        +show_image(image)
        +ordered_listdir(image_dir)
    }
    class image_annotator {
        +draw_detection_results(detection_output_filename, target_dir)
        +write_video(image_dir)
    }
    class imagenet_image_fetcher {
        +bounding_boxes_are_available_for_this_noun(wnid)
        +download_negative_images(wnid, count, target_dir)
        +download_one_random_image(wnid, target_dir)
        +get_random_url(filename)
        +download_images(wnid)
        +download_bounding_boxes(wnid)
        +all_wnids()
    }
    class imagenet {
        +_top_scores(predictions, n_top_scores=100)
        +top_boxed_scores(detection_output_file, n_top_scores=100)
        +_get_noun_id(index)
        +_populate_noun_descriptions()
        +get_description(noun_id)
        +get_noun_id(noun)
    }
    class google_image_fetcher {
    }
    class flags {
        +set_gflags()
    }
    class filter_positive_images {
        +show_image(filename)
    }
    class fetch_positive_images {
    }
    class draw_bounding_boxes {
        +add_line_to_csv(filename, game, outfile)
        +show_image(filename)
        +print_text(text)
        +draw_brush()
        +mark_imprint_boxes(filename)
        +get_done_basenames(csvfile)
    }
    class detector {
        +detect(image_dir, output_filename, caffemodel, deploy_prototxt)
    }
    class cropping_utils {
        +get_crop_box(row, offset, width, height)
        +crop_image_randomly(positive_wnid, src_image_filename, dst_image_filename)
        +log_error(e, width, height, crop_box)
    }
    class create_data_splits {
        +create_train_and_test_splits()
        +symlink(all_dir, train_dir, test_dir, train_count, test_count)
        +create_category_files(stage)
    }
    class config {
        <<static>> N_FRAMES
        <<static>> NUM_FIRST_FRAMES_SKIPPED
        <<static>> NON_MAXIMAL_SUPPRESSION_OVERLAP_THRESHOLD
        <<static>> POSITIVE_PREDICTION_SCORE_THRESHOLD
        <<static>> TOP_PERCENTAGE
    }
    class classifier {
        +classify(image_filename)
    }
    class bounded_box_cropper {
    }
    class accuracy {
        +determine_accuracy(predictions_log_filename)
    }

    video_id_fetcher ..> youtube_crawler : uses
    video_id_fetcher ..> queries : uses
    video_id_fetcher ..> imagenet : uses
    video_fetcher ..> pafy : uses
    video_fetcher ..> re : uses
    verify_lmdb ..> lmdb : uses
    verify_lmdb ..> caffe : uses
    verify_lmdb ..> numpy : uses
    verify_lmdb ..> pylab : uses
    train ..> flags : uses
    train ..> os : uses
    prepare_data ..> flags : uses
    prepare_data ..> os : uses
    prepare_data ..> ROOT : uses
    nms ..> numpy : uses
    nms ..> pandas : uses
    nms ..> config : uses
    models ..> MODELS: uses
    main ..> video_id_fetcher : uses
    main ..> video_fetcher : uses
    main ..> image_utils : uses
    main ..> classifier : uses
    main ..> detector : uses
    main ..> image_annotator : uses
    main ..> models : uses
    main ..> flags : uses
    judge_predictions ..> pygame : uses
    judge_predictions ..> json : uses
    judge_predictions ..> flags : uses
    image_utils ..> os : uses
    image_utils ..> PIL : uses
    image_utils ..> numpy : uses
    image_utils ..> cv2 : uses
    image_utils ..> pylab : uses
    image_utils ..> re : uses
    image_annotator ..> imagenet : uses
    image_annotator ..> nms : uses
    image_annotator ..> image_utils : uses
    image_annotator ..> performance : uses
    image_annotator ..> config : uses
    imagenet_image_fetcher ..> cropping_utils : uses
    imagenet_image_fetcher ..> random : uses
    imagenet_image_fetcher ..> os : uses
    imagenet_image_fetcher ..> subprocess : uses
    imagenet_image_fetcher ..> threading : uses
    imagenet ..> heapq : uses
    imagenet ..> pandas : uses
    imagenet ..> collections : uses
    google_image_fetcher ..> flags : uses
    google_image_fetcher ..> os : uses
    google_image_fetcher ..> yaml : uses
    google_image_fetcher ..> apiclient : uses
    filter_positive_images ..> pygame : uses
    filter_positive_images ..> shutil : uses
    filter_positive_images ..> os : uses
    fetch_positive_images ..> imagenet_image_fetcher : uses
    draw_bounding_boxes ..> gflags : uses
    draw_bounding_boxes ..> csv : uses
    draw_bounding_boxes ..> glob : uses
    draw_bounding_boxes ..> Image : uses
    draw_bounding_boxes ..> pygame : uses
    draw_bounding_boxes ..> os : uses
    detector ..> os : uses
    detector ..> image_utils : uses
    detector ..> config : uses
    cropping_utils ..> datetime : uses
    cropping_utils ..> csv : uses
    cropping_utils ..> PIL : uses
    cropping_utils ..> os : uses
    cropping_utils ..> random : uses
    create_data_splits ..> gflags : uses
    create_data_splits ..> sys : uses
    create_data_splits ..> glob : uses
    create_data_splits ..> os : uses
    create_data_splits ..> imagenet_image_fetcher : uses
    classifier ..> os : uses
    classifier ..> numpy : uses
    bounded_box_cropper ..> gflags : uses
    bounded_box_cropper ..> cropping_utils : uses
    bounded_box_cropper ..> csv : uses
    bounded_box_cropper ..> PIL : uses
    bounded_box_cropper ..> os : uses
    bounded_box_cropper ..> random : uses
    accuracy ..> json : uses
    accuracy ..> gflags : uses

    package "youtube_crawler" #LightBlue {
    }

    package "video_id_fetcher" #LightBlue {
    }

    package "video_fetcher" #LightBlue {
    }

    package "verify_lmdb" #LightBlue {
    }

    package "train" #LightBlue {
    }

    package "queries" #LightBlue {
    }

    package "prepare_data" #LightBlue {
    }

    package "performance" #LightBlue {
    }

    package "nms" #LightBlue {
    }

    package "models" #LightBlue {
    }

    package "main" #LightBlue {
    }

    package "judge_predictions" #LightBlue {
    }

    package "image_utils" #LightBlue {
    }

    package "image_annotator" #LightBlue {
    }

    package "imagenet_image_fetcher" #LightBlue {
    }

    package "imagenet" #LightBlue {
    }

    package "google_image_fetcher" #LightBlue {
    }

    package "flags" #LightBlue {
    }

    package "filter_positive_images" #LightBlue {
    }

    package "fetch_positive_images" #LightBlue {
    }

    package "draw_bounding_boxes" #LightBlue {
    }

    package "detector" #LightBlue {
    }

    package "cropping_utils" #LightBlue {
    }

    package "create_data_splits" #LightBlue {
    }

    package "config" #LightBlue {
    }

    package "classifier" #LightBlue {
    }

    package "bounded_box_cropper" #LightBlue {
    }

    package "accuracy" #LightBlue {
    }

```
