
# 0. Create Fake Data

###### Code
```mysql
drop table if exists aol.events;

create table aol.events (
	timestamp datetime,
	user_id integer,
    event_id varchar(36),
    event_name varchar(12),
    info varchar(42)
);

INSERT INTO aol.events (`timestamp`, `user_id`, `event_id`, `event_name`, `info`) VALUES 
 ('2024-05-19 03:18:07', 23, '2c523b00-88f8-4303-b77d-ab54ef96150c', 'level_change', '{"level": 3}'),
 ('2024-09-04 00:02:57', 34, 'b6a3026a-8d9e-4cbd-b031-7e95ac3de123', 'level_change', '{"level": 4}'),
 ('2021-10-20 06:08:31', 25, 'a5fb2f94-7c75-4f9e-820a-4e90a2ddecd3', 'level_change', '{"level": 7}'),
 ('2022-05-09 00:06:13', 31, '21cebf8b-94e7-4a61-92ba-0869c05543d9', 'play_game', '{"score": 910, "title": "space_crabs"}'),
 ('2021-03-29 01:30:26', 5, '70a289b2-0f14-4cff-abca-ffce6ec2dd6e', 'play_game', '{"score": 656, "title": "super_squids"}'),
 ('2023-01-31 12:55:28', 9, '0452a5b8-ef45-4b9d-8f3f-92fa9577938b', 'level_change', '{"level": 3}'),
 ('2020-11-20 03:19:09', 28, 'ebceeb57-a8ca-4581-a205-d8a8c870c2c5', 'logout', '{}'),
 ('2021-03-22 00:06:13', 40, '35df0855-6709-45c8-a6ee-a4a8ce4ac3bc', 'play_game', '{"score": 700, "title": "super_squids"}'),
 ('2021-01-19 04:34:00', 7, '766d8c15-ed46-47ff-b98d-c7e58f423bce', 'level_change', '{"level": 4}'),
 ('2022-04-09 23:48:31', 8, '641f1de0-79d9-4f4c-9996-aaec65eaeaff', 'level_change', '{"level": 2}'),
 ('2023-05-19 00:08:54', 25, '24e17b5c-e1c0-469c-8de6-3646ed6944fc', 'logout', '{}'),
 ('2020-10-29 17:31:27', 16, '647d175d-5304-45e1-9c9f-ba54aa774555', 'play_game', '{"score": 212, "title": "wheres_my_car"}'),
 ('2022-02-17 18:47:00', 35, '7a68444f-ba5d-4d6b-a52a-0ddbd6cbb7f8', 'logout', '{}'),
 ('2023-02-08 07:03:52', 15, '480f8f3e-590d-481e-92a0-51a5ceb8b268', 'login', '{}'),
 ('2020-10-04 08:07:03', 47, '9eaa49bb-829c-4b88-af7a-0e6a0ba3b277', 'logout', '{}'),
 ('2021-10-02 09:34:29', 39, '39fe547b-7ed0-453f-86bb-502435645339', 'logout', '{}'),
 ('2023-09-26 23:45:35', 38, '4582b76e-4d9e-4376-a489-c323509f0dda', 'play_game', '{"score": 461, "title": "wheres_my_car"}'),
 ('2022-08-21 06:58:03', 44, '513ca61c-f2f0-4964-8e30-b2fa2953dac9', 'level_change', '{"level": 4}'),
 ('2022-12-14 12:16:12', 15, '5bf54e02-3df7-47b8-84d5-dafc2c99211d', 'login', '{}'),
 ('2021-12-22 23:44:47', 47, '96c1dd2c-5d9f-4b9c-8ee1-0e8da4a2b286', 'play_game', '{"score": 956, "title": "wheres_my_car"}'),
 ('2021-12-12 21:14:17', 25, '6e275165-eb54-4845-9103-77044c0a1851', 'logout', '{}'),
 ('2021-05-23 08:34:52', 27, '9bbe6eb0-cad7-4e19-8ae1-765678dc99bf', 'login', '{}'),
 ('2024-06-18 04:45:05', 31, 'a729fa57-676e-45b3-a8bb-18b2e7ef9570', 'level_change', '{"level": 3}'),
 ('2023-09-02 03:22:08', 34, '3d321da5-2aec-4e65-ba5b-143b8805257a', 'login', '{}'),
 ('2023-07-31 05:03:35', 8, '7f92fcaf-8fb3-4c34-999b-fe98d41b8f72', 'login', '{}'),
 ('2023-03-11 21:48:36', 22, '68aa84e5-dff3-45a8-be5b-ff095ebf39f0', 'level_change', '{"level": 1}'),
 ('2021-06-27 02:08:58', 29, '7c160809-db95-4042-bbe7-b95911a71005', 'level_change', '{"level": 3}'),
 ('2021-07-14 17:57:54', 27, '205fb994-b738-4083-8faf-763424f03475', 'logout', '{}'),
 ('2021-07-01 20:20:28', 33, 'a1b9776e-aed2-412f-b5be-10cef00c81a2', 'level_change', '{"level": 8}'),
 ('2021-05-27 23:16:56', 15, '81e9be00-1742-45a1-aa6f-70c5df254e97', 'login', '{}'),
 ('2021-11-18 04:12:13', 12, '11ffa9ef-aded-4bf3-8acf-53f01674cc41', 'play_game', '{"score": 467, "title": "terror_turtles"}'),
 ('2022-04-17 18:11:37', 6, '9ce00180-896b-4efb-87e6-d7b174a2b42c', 'play_game', '{"score": 212, "title": "wheres_my_car"}'),
 ('2021-01-26 15:07:33', 9, '0cb888a9-2035-4280-b785-673f6b4275f5', 'logout', '{}'),
 ('2023-11-24 06:35:01', 25, 'f799cf12-128e-4d45-b4de-8fdb5207baf9', 'login', '{}'),
 ('2023-09-26 11:37:34', 12, '6ede39d4-2062-4a24-81b2-dac71d16985b', 'login', '{}'),
 ('2021-07-14 18:27:57', 27, '92b8a2af-b882-4e23-bc73-c40e0747315b', 'logout', '{}'),
 ('2023-03-12 11:53:16', 13, 'baa2633e-f174-422b-a29f-41236a7e8bb8', 'level_change', '{"level": 3}'),
 ('2024-09-20 13:35:50', 7, 'a466ab7d-2979-48bb-9818-ffb413cf6eed', 'play_game', '{"score": 480, "title": "space_crabs"}'),
 ('2020-11-27 01:33:25', 29, '2e829866-647b-449a-847a-fecba5dc1d2b', 'login', '{}'),
 ('2022-07-16 04:50:37', 7, 'f7cce7c8-6ec2-4d18-af31-edb322fdcd57', 'play_game', '{"score": 768, "title": "space_crabs"}'),
 ('2021-01-10 03:49:36', 49, 'b7fd481c-044f-4d44-9490-20fd8dad2e32', 'level_change', '{"level": 1}'),
 ('2023-07-09 21:08:25', 33, '46a837b5-960d-4d94-81e5-910fc10099e6', 'logout', '{}'),
 ('2024-04-06 15:53:32', 14, 'c1b5beff-2223-499f-8f80-c0ef99798581', 'logout', '{}'),
 ('2024-06-02 02:17:47', 27, 'b63342c2-40b1-40b9-847c-e074d35607ae', 'play_game', '{"score": 161, "title": "wheres_my_car"}'),
 ('2023-04-24 15:10:25', 38, '81835b8a-d0bf-43a2-8df5-6b2b54d9d63c', 'level_change', '{"level": 1}'),
 ('2022-04-19 07:28:05', 13, '8a0d0c68-f05e-45a0-9d28-dabae006febc', 'play_game', '{"score": 521, "title": "space_crabs"}'),
 ('2023-12-06 15:12:18', 26, 'fe7d8bd0-294c-4f2e-a19f-7b4deae8812a', 'login', '{}'),
 ('2021-02-27 07:12:43', 29, '883c3b2b-dc43-46de-bed4-fd215cc0577b', 'login', '{}'),
 ('2021-07-11 04:42:51', 29, '3d1957d6-e277-4f54-87c1-99edd3d5ac59', 'level_change', '{"level": 4}'),
 ('2022-07-17 03:34:22', 46, 'd09b0daf-e38e-402c-958c-927a47a5ac82', 'logout', '{}'),
 ('2021-01-21 17:01:03', 27, '0b1eac4d-8d36-42f8-959e-a72c1c2b7271', 'level_change', '{"level": 9}'),
 ('2024-09-14 09:27:45', 33, 'cff4b5b8-868b-4958-a49c-489de92c2ca7', 'logout', '{}'),
 ('2024-04-28 20:11:19', 35, '38436a8a-6549-40ef-8a66-c8a02fbaf9a5', 'play_game', '{"score": 351, "title": "space_crabs"}'),
 ('2024-05-09 11:31:56', 26, 'db4a4705-1ce8-41c6-8173-6a165c568c1d', 'play_game', '{"score": 144, "title": "space_crabs"}'),
 ('2024-05-04 09:52:38', 26, 'a4a0f4ac-00bc-4906-ad93-8948ba579e07', 'logout', '{}'),
 ('2022-10-10 10:58:32', 13, '230559ee-a6b0-4a02-98a9-3ddbeaf6567b', 'level_change', '{"level": 3}'),
 ('2023-01-18 16:04:10', 18, '4568f80e-22e4-49d1-bf3c-dc525a6993ae', 'level_change', '{"level": 2}'),
 ('2022-08-04 06:30:54', 7, '1c254998-efab-465e-b0d0-b147bd8c2089', 'level_change', '{"level": 2}'),
 ('2022-10-01 15:37:35', 37, 'c429d1f6-2df9-49c1-bccd-fe39dff48a0b', 'login', '{}'),
 ('2023-07-21 07:54:17', 40, '151d2408-482c-4a48-b374-e66be348b80d', 'login', '{}'),
 ('2022-01-23 12:49:39', 5, '3a3acf15-ceaa-4402-ac5a-0b5c2ef81fe3', 'level_change', '{"level": 4}'),
 ('2022-09-17 09:47:27', 1, 'af254a5d-f6dc-4151-9047-c20cffc983b2', 'play_game', '{"score": 356, "title": "super_squids"}'),
 ('2021-09-01 22:21:15', 29, '4a6994f1-b74d-4c63-95a0-9e6f48479dd8', 'play_game', '{"score": 376, "title": "super_squids"}'),
 ('2023-01-29 04:24:34', 11, '6b2ea4fc-d8f9-4ab5-a34d-1c7dec83d3ca', 'login', '{}'),
 ('2021-02-23 10:48:13', 36, '7c860d70-a84b-4117-9da1-fca6c05c9f23', 'logout', '{}'),
 ('2022-01-29 09:59:16', 26, 'ccf72a4b-557f-4040-a38b-229b98f1655c', 'level_change', '{"level": 9}'),
 ('2021-11-20 22:00:56', 13, '6edcf33b-5636-4e19-98e1-e8ecd5041bff', 'level_change', '{"level": 4}'),
 ('2022-04-25 12:30:06', 29, '30762965-77ab-4feb-b9ea-d1f65f415670', 'level_change', '{"level": 8}'),
 ('2024-04-01 21:04:05', 49, '60608bdc-b6eb-40cf-81ce-38b2eced39dd', 'play_game', '{"score": 412, "title": "space_crabs"}'),
 ('2021-06-16 15:40:02', 6, 'a535f36a-cfd7-4d47-bc2b-1c64777467ef', 'logout', '{}'),
 ('2022-03-14 20:26:30', 49, 'ddf94b2b-7abf-4366-91b9-f79592edc5fe', 'logout', '{}'),
 ('2024-07-15 23:54:32', 16, '9b4d4e86-eb82-48f4-923d-a082db6f4ca9', 'play_game', '{"score": 746, "title": "terror_turtles"}'),
 ('2021-08-30 03:16:54', 17, '12ea0c0e-7075-45ed-8fae-b680281ce91c', 'level_change', '{"level": 5}'),
 ('2022-01-25 00:14:22', 49, '34c0008e-b468-4d10-a9e1-e0614b59b477', 'login', '{}'),
 ('2023-04-23 01:21:00', 27, 'e664b59a-a167-4b4b-907a-e45040c8fbfa', 'login', '{}'),
 ('2022-10-27 19:14:37', 14, '2b5a9acc-dc22-4dbd-8530-52568ecd011b', 'logout', '{}'),
 ('2023-03-04 01:02:32', 35, '8641a1f9-c813-4cdc-a04c-0a0fcb9cd1b7', 'logout', '{}'),
 ('2024-05-23 16:15:12', 40, '0d256dbb-cc70-4375-a436-ff5e17ade57f', 'level_change', '{"level": 5}'),
 ('2021-03-07 05:29:43', 45, 'b66a06a2-94e1-4515-8f05-c33280a264a8', 'level_change', '{"level": 3}'),
 ('2023-07-25 14:14:39', 34, 'a7081bd2-e299-4d10-b1fa-b2e445644fec', 'logout', '{}'),
 ('2022-12-19 13:58:26', 36, 'b01b43dc-1cb9-4a15-8c1e-02f812196f31', 'logout', '{}'),
 ('2023-04-12 09:02:27', 3, '01474b74-4b83-474b-b2e2-d5974cec28a9', 'level_change', '{"level": 3}'),
 ('2024-03-18 09:20:37', 46, '19da810d-9826-4256-9471-f38ec558e34a', 'login', '{}'),
 ('2023-10-17 02:25:53', 48, '25140d52-4404-4881-8a93-8d26fea254ca', 'logout', '{}'),
 ('2022-06-20 08:49:27', 7, '5b6440a6-f89b-4d95-9500-8889cc716b8f', 'login', '{}'),
 ('2023-03-13 16:00:38', 38, 'b7924b4f-4624-4933-903d-b3e9ccd6bc36', 'play_game', '{"score": 141, "title": "terror_turtles"}'),
 ('2023-01-12 05:08:40', 15, '2bbe681e-501a-4640-bf3a-e9a811a5052b', 'level_change', '{"level": 8}'),
 ('2021-03-13 05:03:34', 45, '3c7df8b1-4e1b-441d-b0b3-d3c237a49989', 'level_change', '{"level": 7}'),
 ('2024-03-14 01:34:55', 6, '6683104e-66fa-4f1f-b33e-07bb8c68538c', 'login', '{}'),
 ('2023-07-29 23:08:41', 23, '840d97cf-f8b5-41ae-9b8a-ce3d368060f2', 'logout', '{}'),
 ('2023-09-30 10:03:09', 7, '11e0fe19-fcae-483c-ae5b-dae99daad3f5', 'level_change', '{"level": 5}'),
 ('2023-01-10 11:04:22', 22, '10f8132f-57a5-4837-a14a-45d1834c4e89', 'play_game', '{"score": 391, "title": "super_squids"}'),
 ('2024-03-04 03:35:14', 18, 'a0069265-1751-4076-86cd-eb2742b8f25f', 'login', '{}'),
 ('2023-05-07 00:40:33', 16, 'af1b5d48-ffcc-4cd2-9b41-c01873feee5b', 'play_game', '{"score": 648, "title": "super_squids"}'),
 ('2022-05-14 20:59:58', 27, '81136bc0-3800-4260-8b0d-2c9113c776dd', 'play_game', '{"score": 709, "title": "terror_turtles"}'),
 ('2023-02-16 23:58:54', 37, 'c642a8e2-72ab-4e5f-adc3-538d9b292805', 'play_game', '{"score": 540, "title": "wheres_my_car"}'),
 ('2022-11-14 17:23:39', 31, '2b3426fc-4e6a-454c-9e20-15b396a1986a', 'play_game', '{"score": 385, "title": "super_squids"}'),
 ('2024-02-15 05:02:09', 44, 'a7f377f5-b9fa-4b65-9f35-8c3bae8b88f9', 'level_change', '{"level": 4}'),
 ('2023-12-24 22:28:00', 21, 'e3b9dc28-cb56-4a79-8031-7c00e3ab07c5', 'level_change', '{"level": 5}'),
 ('2023-05-11 20:18:22', 31, 'c2154901-1367-4c0c-b249-2cd3ceb148ee', 'login', '{}'),
 ('2024-01-04 10:29:24', 41, 'fb89d33b-a747-4d67-a130-6bfb7938c921', 'logout', '{}'),
 ('2022-03-14 12:12:10', 46, '6ad05cbc-3275-41ef-9c9f-81fc80308b6e', 'play_game', '{"score": 515, "title": "wheres_my_car"}'),
 ('2020-12-22 01:46:18', 46, 'e49eaadb-abac-47be-b579-3c88d1f51067', 'logout', '{}'),
 ('2022-07-29 17:26:15', 37, '2db7ad2b-7ac2-48db-969c-b9a1abe44373', 'play_game', '{"score": 839, "title": "wheres_my_car"}'),
 ('2021-06-26 01:38:14', 6, 'd5945b00-d968-43b3-8d36-1b3b54cac862', 'logout', '{}'),
 ('2021-01-16 18:57:33', 40, '14c86467-488b-4e6c-a092-7deca1ccb480', 'level_change', '{"level": 5}'),
 ('2022-07-01 17:25:37', 28, '5e3a592e-8e30-4e81-96f1-b4d22ac08a11', 'logout', '{}'),
 ('2023-08-26 00:54:53', 50, '7fd16fbf-9934-4398-a6eb-1dc5d0288403', 'login', '{}'),
 ('2022-09-02 11:01:50', 44, 'b4e266d9-ff13-4b0d-b224-67643c74c37d', 'login', '{}'),
 ('2021-08-13 18:44:19', 45, '1e820229-a4ed-4714-86c0-11d756b93924', 'logout', '{}'),
 ('2023-10-17 04:40:49', 50, '8d80509e-aeea-45b7-b860-fff73bdb1d10', 'login', '{}'),
 ('2024-09-01 14:20:08', 22, '38a3e6c1-352d-4039-bced-eddea623f798', 'play_game', '{"score": 226, "title": "super_squids"}'),
 ('2024-09-19 23:15:53', 24, '3c79e931-f5a9-41b2-ab84-abca30f82a4c', 'logout', '{}'),
 ('2020-10-24 16:32:57', 48, 'fd966ac1-1a3b-4b33-b7db-0e4705d6e0af', 'play_game', '{"score": 468, "title": "super_squids"}'),
 ('2024-08-16 07:58:47', 45, '39e97d03-836c-4211-80a7-c68a9bd4a92f', 'level_change', '{"level": 10}'),
 ('2021-12-20 03:32:54', 48, '81e1822f-5726-44f6-8b69-1e250479b9d1', 'play_game', '{"score": 958, "title": "terror_turtles"}'),
 ('2023-08-17 21:34:24', 47, '5feb44e3-b52b-4166-9f6a-7213a3ad58fe', 'logout', '{}'),
 ('2024-09-16 06:16:30', 19, 'edf911ef-10e5-4b72-ba5f-c582f78adcd9', 'logout', '{}'),
 ('2023-05-06 14:27:01', 38, '46726b3b-22b1-41ca-a7b5-713d5e3fe7b6', 'play_game', '{"score": 367, "title": "terror_turtles"}'),
 ('2024-04-02 03:32:37', 38, '91defdf3-a506-4bd0-824d-5238d1428624', 'login', '{}'),
 ('2023-04-21 22:59:59', 38, 'e78c4e92-6014-497c-83f6-6935a55f8631', 'level_change', '{"level": 2}'),
 ('2023-02-08 05:49:28', 24, '98b4e16a-b618-4af6-b1ea-c55bcd70b44f', 'level_change', '{"level": 3}'),
 ('2024-09-02 08:31:10', 45, 'bdc70dae-c3ae-4079-958a-61d1b3631ffd', 'logout', '{}'),
 ('2021-08-08 08:33:31', 39, '2a48e646-0748-4ae5-872b-8cac19d8d8e0', 'login', '{}'),
 ('2023-08-21 20:00:23', 25, 'ca50923e-e894-4ab4-8893-077b46cd5dc9', 'login', '{}'),
 ('2022-04-15 14:14:51', 30, '0dddb565-3e40-4574-8a73-aa98d4425417', 'level_change', '{"level": 6}'),
 ('2022-12-27 09:12:32', 15, '6555c519-ae18-4ec9-babd-f3f9d7248dc3', 'login', '{}'),
 ('2021-04-05 01:43:09', 42, 'b3277707-7bc9-4317-becd-03248a37e29e', 'logout', '{}'),
 ('2023-01-27 03:28:00', 22, '8f976aa6-6b54-4ba9-af5f-fec8e71ec280', 'login', '{}'),
 ('2021-11-06 02:49:43', 21, '47e28134-bbfe-4b50-be91-55c6015d24bf', 'level_change', '{"level": 9}'),
 ('2022-03-10 11:33:59', 26, '6e0e40e2-387c-4c41-b788-37434edf0982', 'level_change', '{"level": 10}'),
 ('2022-06-26 23:03:05', 29, 'cef363a0-2132-47e6-a06c-a2fa4c9c2ae4', 'play_game', '{"score": 130, "title": "space_crabs"}'),
 ('2024-07-17 11:19:30', 40, '3592489f-92f9-4775-a330-9a2c9c7b999c', 'play_game', '{"score": 953, "title": "space_crabs"}'),
 ('2020-10-09 18:21:32', 36, '7e886fe8-3bba-47d3-b7de-884a52d6e53a', 'logout', '{}'),
 ('2021-07-04 20:54:43', 18, '7dff2c15-1eff-4e8f-86d8-3b5d1f8162fe', 'logout', '{}'),
 ('2021-10-31 02:55:01', 29, 'c364c9f8-3cbd-4b62-8327-e2545f965ff0', 'logout', '{}'),
 ('2023-05-15 10:54:26', 29, 'e1b9696a-c9a2-452c-90c3-ea0b4e42376e', 'logout', '{}'),
 ('2020-11-22 08:40:00', 16, 'c4ae3837-1700-4a8b-ab25-70245cb77111', 'play_game', '{"score": 467, "title": "terror_turtles"}'),
 ('2022-10-26 01:31:26', 1, '300e9e5b-4868-4461-ad31-0e73548c96b1', 'play_game', '{"score": 321, "title": "space_crabs"}'),
 ('2023-08-03 05:19:55', 12, 'a87e091c-17f6-4a66-a90c-bb61b74071e2', 'play_game', '{"score": 255, "title": "terror_turtles"}'),
 ('2023-11-30 19:40:16', 45, '1105c1a1-b1a9-48e9-8582-f8a4b642258e', 'play_game', '{"score": 669, "title": "super_squids"}'),
 ('2022-03-18 06:42:01', 31, '4a92ecde-4193-4df1-9338-31ae7af83165', 'level_change', '{"level": 2}'),
 ('2022-04-04 14:28:48', 25, '74dc18df-2b18-4cc9-bb84-db8908b047fa', 'login', '{}'),
 ('2023-07-15 03:46:27', 25, '76e761a7-6bec-4ae3-ad93-a515442c6180', 'login', '{}'),
 ('2020-11-21 19:59:52', 42, '79f90654-6e1b-43e7-9a6d-a4e5a5a67a87', 'play_game', '{"score": 872, "title": "terror_turtles"}'),
 ('2022-05-19 08:34:03', 46, 'adf8def7-e0cc-4b4a-ad93-13bc9e2e042d', 'level_change', '{"level": 7}'),
 ('2023-05-26 13:27:11', 15, 'ec5455c0-7f1e-4c98-b29f-ed53cb15fcf9', 'login', '{}'),
 ('2024-07-21 06:24:44', 37, '439f4a7e-a9f8-4146-a1c6-22f56542e878', 'login', '{}'),
 ('2021-01-09 02:15:02', 28, 'fb68aac0-b83a-42a0-8e5a-ca57fd22c4ab', 'play_game', '{"score": 487, "title": "wheres_my_car"}'),
 ('2023-10-20 09:54:27', 8, 'bdd0f190-f5dd-4b81-b9a6-fee72698a2a4', 'level_change', '{"level": 6}'),
 ('2021-04-06 11:35:59', 44, 'df035569-b0cf-4d95-bccd-54fa99a8d120', 'level_change', '{"level": 2}'),
 ('2024-03-25 00:23:52', 18, 'c0817b2a-f87e-4dbe-96fc-70019f1cd0a8', 'logout', '{}'),
 ('2020-10-22 13:28:52', 2, '46251af7-897b-4ef8-8c08-05343d3eb37f', 'login', '{}'),
 ('2021-09-11 23:49:13', 22, '6c720ee7-1484-4641-a0ba-24458a572170', 'login', '{}'),
 ('2023-10-15 19:46:27', 48, 'b3d8c5f6-b02a-4c95-b573-140be7c241c8', 'logout', '{}'),
 ('2023-10-22 02:30:20', 11, '02c31c1d-a4f7-4501-8cb5-a284bcb457ff', 'level_change', '{"level": 6}'),
 ('2021-04-16 23:49:33', 13, '569a9275-0127-47e0-898c-0d16b7fdb7ee', 'play_game', '{"score": 584, "title": "space_crabs"}'),
 ('2021-03-05 07:03:26', 24, '27eb421c-56cd-4501-b5ef-5972bf0897fc', 'login', '{}'),
 ('2023-08-30 18:02:37', 38, '2e0029f8-6ae5-4f81-bbf8-7e4e164bb831', 'logout', '{}'),
 ('2022-05-16 04:02:08', 23, '4aed909e-80c1-44e5-adbc-36ee12c3bfa5', 'logout', '{}'),
 ('2023-01-22 02:56:40', 24, 'a4f4b5c5-ef57-4b01-9d37-8bc28c06eda8', 'login', '{}'),
 ('2021-12-08 16:24:30', 42, '52cda67f-c9ce-4cef-ad33-4c4e80cfce83', 'logout', '{}'),
 ('2021-04-10 11:12:14', 38, '7e8c7481-e588-463a-8740-54a651371dc5', 'login', '{}'),
 ('2024-04-18 07:30:47', 32, 'e6bceea1-7f82-49f0-8a1f-79c9b746a118', 'play_game', '{"score": 515, "title": "terror_turtles"}'),
 ('2024-08-06 13:51:09', 28, '1005a197-f487-4da6-ae5f-8d23b8663f11', 'level_change', '{"level": 1}'),
 ('2023-09-14 09:41:13', 32, 'e978aa63-e590-4cd9-8302-b513d8c778aa', 'login', '{}'),
 ('2023-11-27 04:35:38', 38, '94b47b42-3776-4dfc-b89b-4161406f9d4c', 'play_game', '{"score": 667, "title": "wheres_my_car"}'),
 ('2022-06-25 03:17:06', 10, 'e7d10c1a-14a0-4875-8b62-0388a1bbe3f0', 'login', '{}'),
 ('2022-06-18 03:53:14', 50, 'add718d9-f4ae-4374-a130-0972e0ef3e24', 'logout', '{}'),
 ('2024-08-07 02:37:52', 10, '5b8b59ef-7aa1-48ea-9fff-506c90cf78e9', 'logout', '{}'),
 ('2023-02-23 08:47:55', 16, 'e16759f0-39d8-44bf-8069-d0dc72acab22', 'logout', '{}'),
 ('2023-12-12 15:00:15', 7, '3e9ab771-069d-4c31-b6c6-d7f714e226fb', 'logout', '{}'),
 ('2021-03-08 19:29:41', 9, '8f6c4b1d-52dc-4708-8cc9-ff549eb431a1', 'play_game', '{"score": 519, "title": "super_squids"}'),
 ('2024-09-04 19:59:52', 9, '0322f804-d3a2-47eb-b3de-5a3f7660c3d7', 'level_change', '{"level": 9}'),
 ('2021-02-13 23:03:59', 46, 'b4a4d729-494a-49f8-9c07-9d090a4a2f3d', 'logout', '{}'),
 ('2023-03-06 20:39:19', 33, '450b725d-c8f0-4c5a-b968-4012808db5ea', 'play_game', '{"score": 654, "title": "terror_turtles"}'),
 ('2022-06-22 13:55:32', 48, '86254600-eaf8-4c86-ac72-af073b62d721', 'play_game', '{"score": 433, "title": "super_squids"}'),
 ('2024-02-09 01:46:43', 28, '34ee5845-78dd-47d7-b829-8aca0fcf2bfb', 'logout', '{}'),
 ('2023-10-31 11:14:27', 4, '73d5b773-4cf0-4b96-9d4b-d3e4a5b651fa', 'login', '{}'),
 ('2023-06-11 20:26:47', 10, '72116926-f92b-484f-9ece-c86e207aa724', 'login', '{}'),
 ('2021-06-25 03:23:11', 32, '1b61b5d2-e13d-4180-9dfc-0360c139d411', 'login', '{}'),
 ('2021-05-03 14:39:44', 36, '26e4917d-2b59-4a27-94b2-5edeb8954e71', 'logout', '{}'),
 ('2023-03-25 18:09:55', 3, 'fe33f6cd-44f1-4e04-8eb5-808d13e0584f', 'level_change', '{"level": 3}'),
 ('2024-04-08 13:19:43', 46, '828416d7-4e20-4e64-979c-47b4d7b64d8d', 'play_game', '{"score": 249, "title": "terror_turtles"}'),
 ('2021-12-22 23:51:34', 10, 'eb28da47-4745-46c2-b477-2b4b92d32482', 'logout', '{}'),
 ('2023-04-29 15:30:48', 40, '90ea44c2-1b55-4a97-8d0a-d02e328c44cb', 'login', '{}'),
 ('2022-10-31 04:17:37', 20, 'f5838f4a-431e-45b5-89f2-d8c98c92ff33', 'login', '{}'),
 ('2021-05-04 20:21:21', 17, '5b9beac3-a5ad-4519-8d4c-b050b87d0683', 'level_change', '{"level": 5}'),
 ('2023-12-16 07:49:17', 31, 'e809b434-674d-4819-a6e6-592c72081c68', 'logout', '{}'),
 ('2022-01-05 00:04:56', 37, '0690b5e8-e925-4100-a553-64639fbada5e', 'logout', '{}'),
 ('2021-05-09 23:24:25', 10, 'cf1b2825-369b-4482-92dc-540bd5b27d25', 'logout', '{}'),
 ('2021-11-21 21:37:01', 50, '1e1c2b0c-72ad-45d4-9cc3-ed872ab97222', 'login', '{}'),
 ('2022-12-02 18:09:09', 34, '8f81733a-cfae-44d5-8b05-4fab1ad17422', 'play_game', '{"score": 577, "title": "super_squids"}'),
 ('2024-01-07 05:21:11', 2, '5a010754-c978-478e-9d67-5f70764325a7', 'level_change', '{"level": 10}'),
 ('2022-06-13 21:37:53', 2, '3b0c8957-fab7-4e51-b765-4264fe768051', 'play_game', '{"score": 992, "title": "terror_turtles"}'),
 ('2024-07-23 19:17:20', 9, '981fe564-94e8-42f4-891b-b82233e4868e', 'level_change', '{"level": 3}'),
 ('2022-08-11 22:44:55', 17, '6aad3279-ced3-4a23-b107-f668fa94dee5', 'logout', '{}'),
 ('2022-12-05 20:10:22', 17, '598c48e5-2348-4eea-9222-88aa8863af45', 'level_change', '{"level": 4}'),
 ('2024-04-22 23:08:33', 7, '1359b881-5497-4301-b5b3-105514d03758', 'play_game', '{"score": 424, "title": "wheres_my_car"}'),
 ('2022-03-01 07:44:02', 15, '4f1af4d9-f28c-4588-8ca5-194d0259efbc', 'logout', '{}');

-- 
select count(*) from aol.events;

select * from aol.events;


```


# Final Submission & Code Analysis
Diagram:
![[Screenshot 2024-09-18 at 17.08.20.png]]

### Final Code
```mysql
-- 6. dim_users
INSERT INTO dim_users (user_id, current_level)
SELECT DISTINCT user_id, 0 AS current_level
FROM events
WHERE event_name IN ('play_game', 'login', 'logout', 'level_change')
ON DUPLICATE KEY UPDATE current_level = COALESCE(dim_users.current_level, 0);

-- update users level
UPDATE dim_users AS du
JOIN (
  SELECT 
  	e.user_id, 
  	SUM(JSON_UNQUOTE(JSON_EXTRACT(e.info, '$.level_change'))) as total_level_change
  	-- 这里假设level_change表示等级变动，也就是如果level_change = 2表示升了2级，所以用SUM
    -- 如果level_change表示当前等级，那可以替换成MAX，表示最大的level_change就是当前等级
  FROM events AS e
  WHERE e.event_name = 'level_change'
  GROUP BY e.user_id
) AS lc ON du.user_id = lc.user_id
SET du.current_level = lc.total_level_change;
```


```mysql
-- 7. dim_games
INSERT INTO dim_games (game_title)
SELECT DISTINCT JSON_UNQUOTE(JSON_EXTRACT(info, '$.title')) AS game_title
FROM events
WHERE event_name = 'play_game';
```


```mysql
-- 8. fact_game_play
INSERT INTO fact_game_play (game_id, user_id, score, game_play_timestamp)
SELECT DISTINCT
	dg.game_id,
    e.user_id,
    JSON_UNQUOTE(JSON_EXTRACT(e.info, '$.score')) AS score,
    e.timestamp
FROM events AS e
JOIN dim_games as dg
ON TRIM(dg.game_title) = TRIM(JSON_UNQUOTE(JSON_EXTRACT(e.info, '$.title')))
WHERE e.event_name = 'play_game';
```


```mysql
-- 9. fact_user_level (keep track of user level over time)
INSERT INTO fact_user_level (user_id, level, level_change_timestamp)
SELECT
	e.user_id,
    -- JSON_UNQUOTE(JSON_EXTRACT(e.info, '$.level_change')) AS level_change, --这样最后得到的只是当前用户当前时间下的等级变动，而不是等级
    SUM(JSON_UNQUOTE(JSON_EXTRACT(e.info, '$.level_change'))) OVER 
    	(PARTITION BY e.user_id ORDER BY e.timestamp ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) AS level,	-- 这样才是当前等级
    e.timestamp
FROM events AS e
WHERE e.event_name = 'level_change';
```


```mysql
-- Answer Analyst's 3 Questions
-- 1. average score for the game terror_turtles for each level of play:
SELECT 
	du.current_level AS level_of_play,
    AVG(fg.score) AS average_score
FROM fact_game_play as fg
JOIN dim_games AS dg ON fg.game_id = dg.game_id
JOIN dim_users as du ON fg.user_id = du.user_id
WHERE dg.game_title = 'terror_turtles'
GROUP BY du.current_level
ORDER BY level_of_play;

-- 2. what game have current level 3 users played the most over all time:
SELECT 
	dg.game_title,
    COUNT(fg.game_id) AS play_count			-- ?? should fg.game_play_id
FROM fact_game_play AS fg
JOIN dim_users AS du ON fg.user_id = du.user_id
JOIn dim_games AS dg ON fg.game_id = dg.game_id
WHERE du.current_level = 3
GROUP BY dg.game_title
ORDER BY play_count DESC;	-- there are tied games

-- 3. what game have users who were at level 3 at the time they played the game played the most over all time?
SELECT 
	dg.game_title,
	COUNT(fg.game_id) AS play_count
FROM fact_game_play AS fg
JOIN dim_games AS dg 
	ON fg.game_id = dg.game_id
JOIN fact_user_level AS fl 
	ON fg.user_id = fl.user_id
WHERE fl.level = 3
AND fg.game_play_timestamp >= fl.level_change_timestamp
GROUP BY dg.game_title
ORDER BY play_count DESC
LIMIT 1;
```






































