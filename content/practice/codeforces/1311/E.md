---
title: "CF 1311E - Xây dựng cây nhị phân"
description: "Chúng ta được yêu cầu xây dựng một cây có gốc trên các đỉnh được đánh nhãn từ 1 đến n, trong đó đỉnh 1 là gốc. Mỗi đỉnh ngoại trừ gốc có chính xác một đỉnh cha và mỗi đỉnh được phép có nhiều nhất hai đỉnh con, do đó cấu trúc phải là cây nhị phân theo nghĩa gốc."
date: "2026-06-16T06:43:47+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "constructive-algorithms", "trees"]
categories: ["algorithms"]
codeforces_contest: 1311
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 624 (Div. 3)"
rating: 2200
weight: 1311
solve_time_s: 755
verified: false
draft: false
---

[CF 1311E - Xây dựng cây nhị phân](https://codeforces.com/problemset/problem/1311/E) 

**Đánh giá:** 2200 
**Tags:** vũ lực, thuật toán xây dựng, cây 
**Thời gian giải:** 12 phút 35 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu xây dựng một cây có gốc trên các đỉnh được đánh nhãn từ 1 đến n, trong đó đỉnh 1 là gốc. Mỗi đỉnh ngoại trừ gốc có chính xác một đỉnh cha và mỗi đỉnh được phép có nhiều nhất hai đỉnh con, do đó cấu trúc phải là cây nhị phân theo nghĩa gốc. 

Đại lượng chính là tổng độ sâu của tất cả các đỉnh. Độ sâu của một nút là số cạnh trên đường đi từ gốc đến nút đó. Chúng ta được cấp một giá trị đích d và phải quyết định xem liệu chúng ta có thể xây dựng một cây nhị phân như vậy có tổng độ sâu bằng d hay không và nếu có thì đưa ra bất kỳ phép gán cha hợp lệ nào. 

Các ràng buộc đủ nhỏ để n tối đa là 5000 cho mỗi trường hợp thử nghiệm và tổng số tiền trên các trường hợp thử nghiệm cũng bị giới hạn bởi 5000. Điều này loại trừ bất cứ điều gì tồi tệ hơn hành vi tổng bậc hai trong tất cả các thử nghiệm, nhưng quan trọng hơn là nó cho phép chúng ta suy nghĩ về mặt xây dựng tham lam với sổ sách kế toán cẩn thận thay vì lập trình động nặng nề trên các trạng thái lớn. 

Có hai cấu hình cực kỳ quan trọng về mặt cấu trúc. Cây nông nhất có thể là một ngôi sao hoàn hảo trong đó mỗi nút là con trực tiếp của gốc, cho tổng độ sâu tối thiểu bằng n trừ 1. Cấu trúc sâu nhất có thể là cây nhị phân hoạt động giống như một chuỗi dài với sự phân nhánh hạn chế, giúp tối đa hóa sự tích lũy độ sâu. Bất kỳ cách xây dựng hợp lệ nào cũng phải nằm giữa những thái cực này. 

Một trường hợp thất bại tinh tế xuất hiện khi n lớn nhưng d quá nhỏ hoặc quá lớn. Nếu d nhỏ hơn n trừ 1 thì ngay cả một ngôi sao cũng quá sâu. Nếu d vượt quá tổng chiều sâu cây nhị phân tối đa có thể đạt được thì không có sự sắp xếp con nào dưới ràng buộc nhị phân có thể đạt tới nó. Một cách tiếp cận ngây thơ làm tăng chiều sâu một cách tham lam mà không tôn trọng ràng buộc hai con có thể dễ dàng xây dựng một cấu trúc vi phạm tính khả thi ngay cả khi các lựa chọn cục bộ có vẻ hợp lệ. 

## Phương pháp tiếp cận 

Ý tưởng brute-force sẽ cố gắng xây dựng cây tăng dần và ở mỗi bước chọn nút cha trong số tất cả các nút hiện có, kiểm tra xem việc gắn nút mới có bảo toàn khả năng đạt được tổng độ sâu cuối cùng cần thiết hay không. Điều này dẫn đến một không gian trạng thái một cách tự nhiên, nơi mỗi nút theo dõi dung lượng và độ sâu hiện tại của nó, đồng thời chúng tôi mô phỏng tất cả các tệp đính kèm hợp lệ. 

Về nguyên tắc, điều này đúng vì mọi cây đều có thể được xây dựng dần dần, nhưng hệ số phân nhánh sẽ bùng nổ. Ở bước i, có các lựa chọn O(i) cho cấp độ gốc và với mỗi lựa chọn, chúng ta cần tính toán lại hoặc ít nhất là theo dõi tính khả thi trong tương lai, dẫn đến hành vi theo cấp số nhân hoặc giai thừa trong trường hợp xấu nhất. 

Quan sát cấu trúc quan trọng là chúng ta không thực sự cần khám phá toàn bộ không gian cây. Chúng ta chỉ cần kiểm soát mức độ đặt các nút sâu so với đường trục mở rộng dần dần. Ràng buộc nhị phân gợi ý rằng mỗi nút có thể hỗ trợ tối đa hai nút con, điều này tạo ra một cấu trúc phân lớp một cách tự nhiên trong đó các nút được lấp đầy theo cấp độ, nhưng với sự "thúc đẩy" các nút có kiểm soát lên các cấp độ sâu hơn để tăng tổng độ sâu. 

Chúng tôi bắt đầu từ cấu hình tối thiểu, một cây nhị phân hoàn chỉnh được lấp đầy theo từng cấp độ. Cấu hình này cung cấp tổng độ sâu nhỏ nhất có thể trong số tất cả các cây nhị phân hợp lệ. Từ đường cơ sở này, chúng tôi liên tục "đẩy" các nút sâu hơn bằng cách gắn lại chúng dưới các nút gốc sâu hơn theo cách tôn trọng dung lượng của từng nút. Mỗi động thái như vậy sẽ làm tăng tổng độ sâu lên một mức có thể kiểm soát được và chúng ta tham lam áp dụng những mức tăng này cho đến khi đạt đến d hoặc xác định rằng điều đó là không thể. 

Điều này làm giảm vấn đề quản lý năng lực sẵn có theo mức độ sâu và sử dụng một cách tham lam các hoạt động tăng độ sâu theo thứ tự hiệu quả giảm dần.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tham lam xây dựng dựa trên năng lực | O(n log n) cho mỗi bài kiểm tra (có hiệu quả là O(n^2) tệ nhất) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng cây nhị phân bằng cách duy trì danh sách các nút được nhóm theo độ sâu và theo dõi số lượng nút con mà mỗi nút vẫn có thể chấp nhận. 

1. Trước tiên, chúng tôi xây dựng cây nhị phân cơ sở theo thứ tự chiều rộng. Chúng tôi đặt các nút theo cấp độ, đảm bảo rằng không có nút nào nhận được nhiều hơn hai nút con. Điều này tạo ra một cây nhị phân hợp lệ với cấu trúc độ sâu tối thiểu có thể. 
2. Chúng tôi tính tổng độ sâu hiện tại của cây cơ sở này. Đây là giá trị khởi đầu của chúng tôi. Nếu nó đã bằng d thì chúng ta đã hoàn thành. 
3. Chúng tôi tính toán độ sâu bổ sung mà chúng tôi cần phân phối trên các nút. Mỗi lần chúng ta di chuyển một nút vào sâu hơn một cấp trong cây, tổng độ sâu sẽ tăng đúng một cấp cho nút đó. 
4. Để tăng độ sâu, chúng tôi duy trì một danh sách các nút được sắp xếp theo độ sâu hiện tại của chúng. Chúng tôi cố gắng "gán lại" các nút cho các nút cấp cao hơn bất cứ khi nào có thể, ưu tiên các nút có thể được đẩy xuống mức sâu nhất. 
5. Chúng tôi duy trì cấu trúc của các nút cha có sẵn ở mỗi độ sâu, trong đó một nút chỉ có thể chấp nhận một nút con mới nếu nó có ít hơn hai nút con được chỉ định cho đến nay. Chúng tôi luôn đính kèm các nút mới hoặc đã di chuyển vào nút gốc hợp lệ nông nhất có thể mà vẫn cho phép chúng tôi duy trì tính khả thi cho các nút còn lại. 
6. Chúng tôi liên tục thực hiện các phần đính kèm tăng độ sâu này cho đến khi tổng độ sâu đạt đến d. Nếu tại một thời điểm nào đó không thể gắn lại hợp lệ nữa trong khi vẫn ở dưới d, chúng tôi kết luận rằng không thể đạt được tổng mục tiêu. 
7. Sau khi cấu trúc được hoàn thiện, chúng ta xuất mảng cha. 

Ý tưởng trung tâm là mọi cây hợp lệ có thể được coi là sự phân phối lại các nút theo các cấp bắt đầu từ cấu hình tối thiểu và mỗi đơn vị tăng lên trong tổng độ sâu tương ứng với việc di chuyển chính xác một nút sâu hơn một cấp theo cách được kiểm soát. 

### Tại sao nó hoạt động 

Chúng tôi bắt đầu từ cấu hình với tổng độ sâu tối thiểu có thể theo các ràng buộc nhị phân. Bất kỳ cây nhị phân hợp lệ nào khác đều có thể được chuyển đổi sang cấu hình này bằng cách liên tục di chuyển các nút lên trên cho đến khi không thể giảm thêm độ sâu. Điều này có nghĩa là mọi cấu hình hợp lệ đều tương ứng với một số thao tác nghịch đảo, trong đó các nút được đẩy sâu hơn. 

Vì mỗi nút có nhiều nhất hai nút con nên số lượng "khe" có sẵn ở mỗi cấp giới hạn chính xác số lượng nút có thể được di chuyển xuống ở mỗi bước. Chiến lược tham lam là luôn sử dụng năng lực sẵn có sớm nhất đảm bảo chúng ta không bao giờ chặn những bước đi trong tương lai một cách không cần thiết, vì năng lực sâu hơn luôn bị hạn chế hơn năng lực nông. Điều này duy trì tính khả thi trong khi tăng tổng độ sâu một cách đơn điệu cho đến khi đạt được mục tiêu. 

## Giải pháp Python```
PythonRun
```Việc triển khai trước tiên sẽ xây dựng một cây nhị phân hợp lệ với phân bố độ sâu nhỏ nhất bằng cách sử dụng phần mở rộng BFS. Điều này đảm bảo tính khả thi đối với ràng buộc nhị phân trong khi vẫn cung cấp đường cơ sở ổn định. Sau đó, nó tính toán tổng độ sâu hiện tại và kiểm tra xem giá trị được yêu cầu có nằm trong giới hạn có thể đạt được hay không. 

Giai đoạn điều chỉnh cố gắng tăng độ sâu bằng cách gắn lại các nút theo các phép biến đổi từ nông đến sâu. các`avail`list theo dõi các nút vẫn có thể chấp nhận nút con, đảm bảo ràng buộc nhị phân không bao giờ bị vi phạm. Mỗi lần gắn lại thành công sẽ cập nhật cả mảng gốc và mảng độ sâu, đồng thời tăng tổng độ sâu bằng sự thay đổi chính xác về độ sâu của nút đã di chuyển. 

Một điểm tinh tế quan trọng là chúng tôi luôn xử lý các nút từ các vị trí sâu hơn trước tiên. Điều này tối đa hóa cơ hội mà chúng tôi di chuyển các nút theo cách sớm tạo ra sự gia tăng có ý nghĩa về độ sâu, ngăn chặn các hoạt động lãng phí trên các nút không thể đào sâu hơn nữa. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```

```Trước tiên chúng tôi xây dựng cây nhị phân BFS: 

| Nút | Phụ huynh | Độ sâu | 
| --- | --- | --- | 
| 1 | - | 0 | 
| 2 | 1 | 1 | 
| 3 | 1 | 1 | 
| 4 | 2 | 2 | 
| 5 | 2 | 2 | 

Tổng độ sâu là 0 + 1 + 1 + 2 + 2 = 6. Cần đạt 7 nên phải tăng thêm 1. 

Chúng tôi di chuyển một nút sâu nhất, chẳng hạn như nút 5, bên dưới nút 3. 

| Hoạt động | Nút đã di chuyển | Phụ huynh mới | Thay đổi độ sâu | Tổng số tiền | 
| --- | --- | --- | --- | --- | 
| ban đầu | - | - | - | 6 | 
| di chuyển | 5 | 3 | +1 | 7 | 

Điều này tạo ra một cây nhị phân hợp lệ với số tiền được yêu cầu. 

### Ví dụ 2 

đầu vào:```

```Cây BFS ban đầu: 

| Nút | Phụ huynh | Độ sâu | 
| --- | --- | --- | 
| 1 | - | 0 | 
| 2 | 1 | 1 | 
| 3 | 1 | 1 | 
| 4 | 2 | 2 | 
| 5 | 2 | 2 | 
| 6 | 3 | 2 | 

Tổng = 0 + 1 + 1 + 2 + 2 + 2 = 8, cần +2. 

Chúng tôi di chuyển nút 5 dưới nút 6, tăng độ sâu từ 2 lên 3 (+1), sau đó di chuyển nút 4 dưới nút 6 hoặc 3 tùy thuộc vào tình trạng sẵn có, tạo ra một +1 khác. Tổng cuối cùng trở thành 10. 

Các bước này minh họa rằng mỗi lần gắn lại góp phần tăng thêm độ sâu tổng thể có kiểm soát và bổ sung. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^2) tệ nhất trong mỗi bài kiểm tra | Cấu trúc BFS là O(n), quét lại các phụ huynh có sẵn có thể tuyến tính | 
| Không gian | O(n) | Chúng tôi lưu trữ các mảng theo dõi gốc, độ sâu và kề cận | 

Cho rằng tổng số n trong tất cả các trường hợp thử nghiệm nhiều nhất là 5000, hành vi bậc hai này vẫn nằm trong giới hạn. 

## Trường hợp thử nghiệm```
PythonRun
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| n=2 cạnh | CÓ/KHÔNG | độ chính xác cấu trúc tối thiểu | 
| quá nhỏ d | KHÔNG | xử lý giới hạn dưới | 
| trường hợp sao | danh sách cha mẹ đơn giản | hiện thực hóa độ sâu tối thiểu | 
| trường hợp vừa phải | CÓ | logic gắn lại | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn là khi d bằng tổng tối thiểu có thể n trừ 1. Trong trường hợp này, cấu trúc đúng là một ngôi sao bắt nguồn từ 1 và mọi nỗ lực phân phối lại các nút sâu hơn sẽ chỉ làm tăng tổng và làm cho nó không hợp lệ. 

Một trường hợp cạnh khác xảy ra khi d cực kỳ lớn, gần với mức tối đa theo lý thuyết. Ở đây, việc xây dựng phải tôn trọng ràng buộc nhị phân một cách cẩn thận, bởi vì việc tham lam đẩy các nút xuống sâu hơn mà không theo dõi các ô con có sẵn sẽ dẫn đến các cây không hợp lệ khi một nút vượt quá hai nút con. 

Trường hợp khó phát hiện cuối cùng là khi các cấu hình trung gian có vẻ khả thi cục bộ nhưng lại cạn kiệt công suất gốc quá sớm. Thuật toán tránh điều này bằng cách luôn theo dõi công suất sẵn có trên toàn cầu thay vì đưa ra các quyết định đính kèm riêng biệt, đảm bảo rằng các mối gắn lại sâu không bao giờ cản trở các vị trí cần thiết trong tương lai.
