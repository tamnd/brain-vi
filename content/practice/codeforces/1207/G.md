---
title: "CF 1207G - Album Indie"
description: "Chúng ta có một tập hợp các chuỗi ngày càng tăng, trong đó mỗi chuỗi mới là một ký tự đơn hoặc một chuỗi cũ được mở rộng thêm chính xác một ký tự ở cuối."
date: "2026-06-15T17:48:26+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "dfs-and-similar", "hashing", "string-suffix-structures", "strings", "trees"]
categories: ["algorithms"]
codeforces_contest: 1207
codeforces_index: "G"
codeforces_contest_name: "Educational Codeforces Round 71 (Rated for Div. 2)"
rating: 2700
weight: 1207
solve_time_s: 250
verified: false
draft: false
---

[CF 1207G - Album Indie](https://codeforces.com/problemset/problem/1207/G) 

**Xếp hạng:** 2700 
**Thẻ:** cấu trúc dữ liệu, dfs và tương tự, băm, cấu trúc hậu tố chuỗi, chuỗi, cây 
**Thời gian giải:** 4 phút 10 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một tập hợp các chuỗi ngày càng tăng, trong đó mỗi chuỗi mới là một ký tự đơn hoặc một chuỗi cũ được mở rộng thêm chính xác một ký tự ở cuối. Điều này có nghĩa là toàn bộ hệ thống tạo thành một cây có gốc: mỗi chuỗi trỏ đến cha của nó (chuỗi được tạo từ đó) và mỗi cạnh nối thêm một ký tự. 

Đối với mỗi truy vấn, chúng tôi được yêu cầu lấy một trong các chuỗi này và đếm số lần một chuỗi khác xuất hiện dưới dạng chuỗi con liền kề bên trong nó. 

Khó khăn chính là các chuỗi không được cung cấp đầy đủ một cách rõ ràng. Một số trong số chúng có thể cực kỳ dài và nhiều truy vấn có thể tham chiếu đến các tiền tố giống nhau hoặc chồng chéo của cây ẩn này. Vì tổng kích thước đầu vào lớn nên bất kỳ cách tiếp cận nào xây dựng rõ ràng tất cả các chuỗi hoặc quét chúng cho mỗi truy vấn đều không khả thi ngay lập tức. 

Các ràng buộc ngụ ý lên tới 400.000 nút và 400.000 truy vấn, với tổng độ dài truy vấn cũng bị giới hạn bởi 400.000. Tìm kiếm chuỗi con đơn giản cho mỗi truy vấn sẽ hoạt động như sau$O(n \cdot |t|)$trong trường hợp xấu nhất, vượt quá giới hạn. 

Một vấn đề tế nhị nảy sinh từ việc tái sử dụng cấu trúc. Nhiều chuỗi chia sẻ tiền tố dài vì mỗi chuỗi được hình thành bằng cách thêm một ký tự. Ví dụ: nếu chuỗi 10 được xây dựng từ chuỗi 5 thì chuỗi 10 chứa mọi thứ trong 5 cộng với một ký tự phụ. Bất kỳ giải pháp nào cũng phải khai thác việc tái sử dụng này thay vì xây dựng lại hoặc quét từ đầu. 

Một cạm bẫy khác là xử lý từng chuỗi một cách độc lập. Nếu chúng ta lưu trữ các chuỗi một cách rõ ràng, bộ nhớ và thời gian sẽ tăng vọt. Ngay cả việc băm tất cả các chuỗi con trên mỗi nút cũng quá lớn vì một chuỗi có độ dài L có các chuỗi con O(L²). 

## Phương pháp tiếp cận 

Phương pháp bạo lực sẽ xây dựng mọi chuỗi một cách rõ ràng và sau đó, đối với mỗi truy vấn, quét chuỗi mục tiêu và đếm các kết quả khớp của mẫu bằng cửa sổ trượt. Điều này đúng về mặt logic nhưng không thành công vì một chuỗi có thể rất dài và có thể có nhiều truy vấn. Ngay cả với KMP, mỗi truy vấn đều có giá$O(|s_i| + |t|)$và vì cả hai đều có thể lớn trong nhiều truy vấn, nên tổng chiều dài của trường hợp xấu nhất sẽ trở thành bậc hai. 

Quan sát cấu trúc quan trọng là tất cả các chuỗi tạo thành một cây trong đó mỗi nút khác với nút gốc của nó bởi một ký tự được nối thêm. Điều này có nghĩa là mọi chuỗi tương ứng với một đường dẫn từ gốc và các chuỗi con tương ứng với các đường dẫn trong cây này không nhất thiết phải là các đoạn liên tục nhưng vẫn liên tục. 

Thay vì coi các chuỗi là các chuỗi phẳng, chúng tôi diễn giải lại toàn bộ cấu trúc dưới dạng cấu trúc giống như bộ ba được tăng cường bằng các liên kết hậu tố. Hướng tự nhiên là xây dựng máy tự động Aho-Corasick trên tất cả các chuỗi truy vấn, vì các truy vấn là các mẫu cố định và sau đó quét từng chuỗi được tạo một lần thông qua máy tự động. 

Tuy nhiên, việc quét từng chuỗi đầy đủ riêng lẻ vẫn còn quá tốn kém. Cải tiến quan trọng là quan sát thấy rằng các chuỗi được xây dựng tạo thành một cây với các con trỏ cha, vì vậy chúng ta có thể duyệt cây này trong khi duy trì trạng thái máy tự động, tích lũy các lần xuất hiện mẫu khi chúng ta di chuyển. Điều này biến việc đếm chuỗi con thành một vấn đề lan truyền trên cây chuỗi. 

Mỗi nút mang một sự chuyển đổi ký tự từ nút cha của nó. Chúng tôi duy trì cho mỗi nút trạng thái máy tự động hiện tại đạt được sau khi đọc chuỗi của nó và chúng tôi truyền bá các đóng góp bằng cách sử dụng DFS trên cây xây dựng. Mỗi lần chúng tôi nhập một nút, chúng tôi sẽ cập nhật số lượng mẫu truy vấn kết thúc ở trạng thái tự động hóa đó. 

Điều này giúp giảm việc quét lặp lại và đảm bảo mỗi nút được xử lý một lần, trong khi quá trình chuyển đổi tự động xử lý tất cả các mẫu khớp một cách hiệu quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Quét lực lượng vũ phu cho mỗi truy vấn | (O(n \cdot | t | )) | 
| Cây + Aho-Corasick + nhân giống DFS | (O(n + \sum | t | )) | 

## Hướng dẫn thuật toán 

Chúng tôi tách vấn đề thành hai cấu trúc tương tác: cây bài hát và máy tự động được xây dựng từ tất cả các mẫu truy vấn. 

1. Xây dựng máy tự động Aho-Corasick từ tất cả các chuỗi truy vấn. Mỗi chuỗi truy vấn được chèn vào bộ ba và mỗi nút đầu cuối sẽ lưu trữ những truy vấn nào kết thúc ở đó. Sau đó, chúng tôi tính toán các liên kết hậu tố để mọi nút đều biết nơi cần quay lại khi quá trình chuyển đổi ký tự không thành công. 
2. Đối với mỗi nút tự động, hãy tính toán danh sách đầu ra của nó bằng cách hợp nhất các kết quả khớp của chính nó với các kết quả của liên kết hậu tố của nó. Điều này đảm bảo rằng khi chúng tôi đạt đến một trạng thái, chúng tôi sẽ biết ngay mẫu truy vấn nào kết thúc ở đó. 
3. Xây dựng cây bài hát. Mỗi nút i lưu trữ nút cha j (nếu là loại 2) và ký tự được nối thêm của nó. 
4. Thực hiện DFS trên cây bài hát đồng thời duy trì trạng thái máy tự động tương ứng với chuỗi hiện tại. 
5. Tại thư mục gốc, bắt đầu với máy tự động ở trạng thái ban đầu (chuỗi trống). Đối với mỗi nút, chuyển đổi bằng cách sử dụng ký tự của nó từ trạng thái gốc. 
6. Khi nhập một nút, chúng ta đang ở cuối chuỗi đầy đủ của nút đó. Chúng tôi lặp lại tất cả các mẫu kết thúc ở trạng thái máy tự động hiện tại và tăng bộ đếm câu trả lời của chúng. 
7. Tái diễn thành trẻ em, chuyển trạng thái máy tự động được cập nhật. 

Điểm tinh tế là trạng thái tự động luôn biểu thị cấu trúc hậu tố của chuỗi hiện tại. Điều này tránh việc tính toán lại các kết quả khớp cho các chuỗi con chồng chéo vì các liên kết hậu tố ngầm mã hóa tất cả các kết quả khớp dự phòng có thể có. 

### Tại sao nó hoạt động 

Tại mỗi nút trong DFS, trạng thái máy tự động tương ứng chính xác với tập hợp tất cả các hậu tố của chuỗi hiện tại khớp với tiền tố của bất kỳ mẫu truy vấn nào. Mỗi lần xuất hiện của mẫu kết thúc tại nút đó phải tương ứng với một trong các trạng thái tự động hóa này. Vì Aho-Corasick hợp nhất các đóng góp hậu tố nên mỗi kết quả trùng khớp được tính chính xác một lần khi DFS truy cập vào nút nơi mẫu kết thúc. 

## Giải pháp Python```
PythonRun
```Giai đoạn xây dựng xây dựng máy tự động Aho-Corasick tiêu chuẩn trên tất cả các chuỗi truy vấn. Mỗi nút trong máy tự động lưu trữ các mẫu kết thúc ở đó và các liên kết hậu tố hợp nhất các lần xuất hiện để một trạng thái duy nhất đại diện cho tất cả các mẫu phù hợp kết thúc tại vị trí đó. 

DFS đi qua cây xây dựng bài hát. Chi tiết triển khai chính là trạng thái tự động hóa được xâu chuỗi thông qua đệ quy thay vì được tính toán lại trên mỗi nút. Mỗi quá trình chuyển đổi nút là một phần mở rộng ký tự duy nhất của trạng thái gốc của nó, với dự phòng thông qua các liên kết hậu tố khi cần. 

Một cạm bẫy phổ biến là quên truyền bá đầu ra dọc theo các liên kết hậu tố. Không sáp nhập`out[link[u]]`vào trong`out[u]`, nhiều kết quả trùng khớp bị bỏ lỡ vì các mẫu kết thúc ở trạng thái hậu tố sẽ không được tính ở các nút sâu hơn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 (đơn giản hóa) 

Hãy xem xét một chuỗi các chuỗi:`a → ab → aba`và một truy vấn yêu cầu mẫu`"aba"`ở nút cuối cùng. 

Ở mỗi bước DFS, chúng tôi theo dõi trạng thái tự động hóa: 

| Nút | Chuỗi | Chuyển đổi trạng thái | Kết quả được tìm thấy | 
| --- | --- | --- | --- | 
| 1 | một | đi đến 'a' | không | 
| 2 | ab | 'a' → 'b' | không | 
| 3 | aba | 'ab' → 'a' | mẫu kết thúc | 

Điều này cho thấy các kết quả khớp chỉ xuất hiện khi trạng thái máy tự động đạt đến nút đầu cuối, không nhất thiết phải ở mọi bước. 

### Ví dụ 2 (các mẫu chồng chéo) 

Dây:`a → aa → aaa`, mẫu`"aa"`. 

| Nút | Chuỗi | Tiểu bang | Trận đấu | 
| --- | --- | --- | --- | 
| 1 | một | "một" | 0 | 
| 2 | aa | "aa" | 1 | 
| 3 | aaa | "aaa" | 2 | 

Điều này chứng tỏ rằng các lần xuất hiện chồng chéo được xử lý một cách tự nhiên bằng cách truyền hậu tố tự động. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n + \sum | t | 
| Không gian | (O(n + \sum | t | 

Giải pháp này phù hợp một cách thoải mái vì cả cấu trúc tổng thể của bài hát và tổng chiều dài truy vấn đều bị giới hạn bởi 400.000, khiến cho việc xử lý theo thời gian tuyến tính trở nên khả thi. 

## Trường hợp thử nghiệm```
PythonRun
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| nút đơn tối thiểu | 0/1 trường hợp | độ đúng cơ sở | 
| lặp lại chuỗi | nhiều lần chồng chéo | truyền bá hậu tố | 
| truy vấn mẫu lặp lại | tích lũy | nhóm truy vấn | 

## Vỏ cạnh 

Một trường hợp đặc biệt quan trọng là khi nhiều mẫu có chung hậu tố dài. Ví dụ, các mẫu`"a"`,`"aa"`,`"aaa"`tất cả đều kết thúc ở trạng thái tự động chồng chéo. Không có hậu tố sáp nhập vào`out[]`, chỉ những mẫu sâu nhất mới được tính, trong khi những mẫu ngắn hơn sẽ bị bỏ qua mặc dù chúng xuất hiện ở mọi vị trí. 

Một trường hợp khác phát sinh khi một bài hát được xây dựng bằng cách thêm nhiều lần cùng một ký tự. Trong trường hợp này, các quá trình chuyển đổi tự động liên tục chạm vào các liên kết dự phòng và tính chính xác phụ thuộc vào việc xâu chuỗi các quá trình chuyển tiếp hậu tố một cách chính xác. Nếu dự phòng không được xử lý, trạng thái sẽ không chính xác và số lượng chuỗi con sẽ bị thu gọn. 

Cả hai trường hợp đều được xử lý vì mọi trạng thái máy tự động đều kế thừa kết quả đầu ra từ liên kết hậu tố của nó, đảm bảo rằng mọi lần xuất hiện mẫu hợp lệ đều được đăng ký chính xác tại nút nơi ký tự cuối cùng của nó được xử lý.
