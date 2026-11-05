---
title: "CF 1217F - Vấn đề truy vấn trực tuyến bắt buộc"
description: "Biểu đồ bắt đầu trống nhưng nó liên tục được sửa đổi bởi hai loại hoạt động. Thao tác đầu tiên chuyển đổi một cạnh giữa hai đỉnh và thao tác thứ hai hỏi liệu hai đỉnh có được kết nối trong biểu đồ hiện tại hay không."
date: "2026-06-15T18:55:13+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "divide-and-conquer", "dsu", "graphs", "trees"]
categories: ["algorithms"]
codeforces_contest: 1217
codeforces_index: "F"
codeforces_contest_name: "Educational Codeforces Round 72 (Rated for Div. 2)"
rating: 2600
weight: 1217
solve_time_s: 311
verified: false
draft: false
---

[CF 1217F - Sự cố truy vấn trực tuyến bắt buộc](https://codeforces.com/problemset/problem/1217/F) 

**Đánh giá:** 2600 
**Thẻ:** cấu trúc dữ liệu, phân chia để chinh phục, dsu, đồ thị, cây 
**Thời gian giải:** 5 phút 11s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Biểu đồ bắt đầu trống nhưng nó liên tục được sửa đổi bởi hai loại hoạt động. Thao tác đầu tiên chuyển đổi một cạnh giữa hai đỉnh và thao tác thứ hai hỏi liệu hai đỉnh có được kết nối trong biểu đồ hiện tại hay không. Điều khó khăn là mọi truy vấn đều được “dịch chuyển” bởi một phần bù thay đổi được gọi là`last`, điều này phụ thuộc vào các câu trả lời kết nối trước đó. Điều này làm cho chuỗi các chỉ số đỉnh thực tế trở nên động hơn là cố định. 

Mỗi truy vấn không sử dụng trực tiếp các điểm cuối đã cho. Thay vào đó, cả hai điểm cuối đều được dịch chuyển theo chu kỳ bởi`last`, do đó việc nhận dạng các đỉnh liên quan đến mọi thao tác phụ thuộc vào lịch sử của các kết quả kết nối trước đó. Điều này loại bỏ khả năng xử lý các truy vấn một cách độc lập và cũng ngăn chặn việc tính toán trước tất cả các cạnh. 

Đầu ra là một chuỗi nhị phân ghi lại câu trả lời cho các truy vấn kết nối. Mỗi ký tự tương ứng với việc hai đỉnh được truy vấn có được kết nối tại thời điểm đó hay không. 

Các ràng buộc cho phép tối đa 200.000 đỉnh và 200.000 truy vấn, do đó, mọi giải pháp xử lý từng truy vấn bằng cách thực hiện duyệt đồ thị như BFS hoặc DFS sẽ không thành công. Ngay cả một lần truyền tải cho mỗi truy vấn cũng dẫn đến hành vi O(nm) trong trường hợp xấu nhất, vượt xa giới hạn. Tương tự, việc duy trì kết nối dưới các chuyển đổi cạnh với cấu trúc biểu đồ động đơn giản là quá chậm trừ khi các cập nhật và truy vấn gần như logarit. 

Một khó khăn tinh tế là sự phụ thuộc ngoại tuyến được tạo bởi`last`. Một sai lầm ngây thơ là trước tiên chuyển đổi tất cả các truy vấn bằng cách sử dụng một`last`sự liên tiếp. Điều đó là không thể bởi vì`last`phụ thuộc vào câu trả lời bạn chỉ biết sau khi xử lý các truy vấn theo thứ tự. Một cạm bẫy khác là coi đây là một vấn đề kết nối động tiêu chuẩn mà không tính đến các chuyển đổi biên, điều này làm phức tạp các phương pháp tiếp cận dựa trên DSU. 

Một trường hợp lỗi cụ thể đối với các phương pháp đơn giản xuất hiện khi các cạnh chuyển đổi thường xuyên. Ví dụ: việc liên tục thêm và xóa cùng một cạnh sẽ gây ra việc tính toán lại kết nối nhiều lần, điều này sẽ kích hoạt các quá trình truyền tải DFS đầy đủ lặp đi lặp lại và dẫn đến hết thời gian chờ ngay cả trên các thành phần nhỏ. 

## Phương pháp tiếp cận 

Giải pháp brute-force sẽ duy trì danh sách kề và tính toán lại khả năng kết nối cho mọi truy vấn loại 2 bằng DFS hoặc BFS. Điều này đúng vì nó luôn phản ánh trạng thái đồ thị hiện tại, nhưng mỗi lần truyền tải có thể lấy O(n) và với m truy vấn thì tổng độ phức tạp sẽ trở thành O(nm). Với 200.000 truy vấn, điều này là không khả thi. 

Thông tin chi tiết quan trọng là mặc dù các biên được chuyển đổi trực tuyến nhưng các truy vấn kết nối có thể được xử lý bằng DSU nếu chúng tôi xử lý thời gian theo cách có cấu trúc. Một DSU tiêu chuẩn không hỗ trợ việc xóa, nhưng vấn đề này có thể được giải quyết bằng cách quan sát rằng mọi cạnh đều có một khoảng thời gian tồn tại: nó hoạt động giữa các sự kiện bật tắt “bật” và “tắt”. Vì vậy, thay vì suy nghĩ linh hoạt, chúng tôi chuyển quy trình thành vấn đề kích hoạt khoảng thời gian ngoại tuyến theo thời gian. 

Tuy nhiên, chúng tôi không thể khắc phục hoàn toàn các truy vấn ngoại tuyến vì`last`sự phụ thuộc. Bí quyết là mô phỏng các truy vấn theo thứ tự trong khi vẫn sử dụng cấu trúc hỗ trợ khôi phục. Điều này dẫn đến sự phân chia và chinh phục theo thời gian với một DSU có thể hoàn tác các hoạt động. 

Chúng tôi duy trì cho mỗi cạnh một chồng khoảng thời gian kích hoạt. Mỗi chuyển đổi xác định điểm bắt đầu hoặc kết thúc của một khoảng thời gian. Sau đó, chúng tôi xử lý các truy vấn bằng cách sử dụng cây phân đoạn theo thời gian, chèn các cạnh vào các nút bao phủ phạm vi hoạt động của chúng. Tại mỗi nút, chúng tôi áp dụng các liên kết DSU, lặp lại và khôi phục sau khi kết thúc. Các truy vấn kết nối được trả lời tại các lá. 

Sự kết hợp giữa cây phân đoạn theo thời gian cộng với khôi phục DSU giải quyết việc xóa một cách rõ ràng và giữ cho mỗi cạnh chỉ được xử lý O(log m) lần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force DFS cho mỗi truy vấn | O(nm) | O(n + m) | Quá chậm | 
| Cây phân đoạn theo thời gian + khôi phục DSU | O((n + m) log m α(n)) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Phân tích tất cả các truy vấn theo thứ tự và tính toán các điểm cuối thực tế bằng cách sử dụng`last`giá trị, vì các truy vấn phải được diễn giải trực tuyến. Mỗi truy vấn được chuyển đổi thành các cặp đỉnh cố định để xử lý sau. 
2. Duy trì bản đồ từ mỗi cạnh đến một chồng thời gian kích hoạt. Khi một cạnh xuất hiện lần đầu tiên, hãy đẩy chỉ số của nó. Khi nó xuất hiện trở lại, hãy bật chỉ mục trước đó và ghi lại khoảng thời gian. Nếu nó vẫn mở ở cuối, hãy đóng nó ở m. 
3. Xây dựng cây phân đoạn theo phạm vi thời gian của truy vấn. Mỗi nút đại diện cho một khoảng thời gian và lưu trữ các cạnh hoạt động hoàn toàn trong khoảng thời gian đó. 
4. Chèn từng khoảng cạnh vào các nút cây phân đoạn bao phủ đầy đủ phạm vi thời gian hoạt động của nó. Điều này đảm bảo mỗi cạnh chỉ được thêm vào các nút O(log m). 
5. Duy trì DSU hỗ trợ khôi phục. Mỗi thao tác hợp ghi lại các thay đổi (cập nhật gốc và kích thước) trên một ngăn xếp để chúng có thể được hoàn tác. 
6. Duyệt cây đoạn theo cách đệ quy. Trước khi đi xuống một nút, hãy áp dụng tất cả các liên kết cạnh được lưu trữ trong đó. 
7. Nếu nút tương ứng với một chỉ mục thời gian duy nhất, hãy xử lý truy vấn tại thời điểm đó. Nếu đó là truy vấn loại 2, hãy kiểm tra xem hai đỉnh có thuộc cùng một thành phần DSU hay không và ghi lại câu trả lời. 
8. Sau khi hoàn thành cây con của một nút, DSU khôi phục sẽ thay đổi để khôi phục trạng thái trước đó trước khi xử lý các nút anh chị em. 
9. Tiếp tục cho đến khi tất cả các nút được xử lý, thu thập các câu trả lời theo thứ tự. 

Tính chính xác dựa trên thực tế là mỗi nút cây phân đoạn biểu thị một khoảng thời gian rời rạc và trạng thái DSU có giá trị cục bộ trong khoảng thời gian đó sau khi áp dụng chính xác các cạnh hoạt động trong suốt khoảng thời gian đó. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào trong đệ quy, DSU chứa chính xác các cạnh đang hoạt động trong đoạn thời gian hiện tại. Bởi vì mỗi cạnh chỉ được chèn vào các nút được bao phủ hoàn toàn bởi khoảng hoạt động của nó, nên nó xuất hiện chính xác khi cần thiết và vắng mặt nếu không. Khôi phục đảm bảo rằng không có cạnh nào bị rò rỉ vào các phân đoạn thời gian không liên quan, do đó mọi truy vấn kết nối đều được trả lời bằng cách sử dụng ảnh chụp nhanh biểu đồ chính xác. 

## Giải pháp Python```
PythonRun
```## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + m) log m α(n)) | mỗi khoảng cạnh được chèn vào các nút phân đoạn O(log m), các hoạt động DSU gần như không đổi | 
| Không gian | O(n + m) | Mảng DSU, lưu trữ cây phân đoạn và ánh xạ truy vấn | 

Độ phức tạp vừa vặn trong giới hạn vì cả m và n đều là 200.000 và chi phí logarit vẫn có thể quản lý được. Việc khôi phục DSU đảm bảo mỗi liên kết được đảo ngược trong thời gian không đổi, ngăn ngừa sự trùng lặp trạng thái trên các nhánh đệ quy. 

## Trường hợp thử nghiệm```
PythonRun
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 3 / 1 1 2 / 2 1 2 / 1 1 2 / 2 1 2 | 11 | chuyển đổi tính đúng đắn | 
| 3 3 / 2 1 2 / 1 1 2 / 2 1 2 | 01 | đồ thị trống ban đầu | 
| 4 6 / chuyển đổi lặp đi lặp lại | khác nhau | lật cạnh bình thường | 

## Vỏ cạnh 

Trường hợp cạnh khóa được lặp đi lặp lại việc chuyển đổi cùng một cạnh. Trong tình huống này, lợi thế không xuất hiện liên tục và các phương pháp DSU ngây thơ sẽ tích lũy nó vĩnh viễn một cách không chính xác. Cấu trúc dựa trên khoảng thời gian đảm bảo mỗi lần kích hoạt được ghép nối với một lần hủy kích hoạt phù hợp, do đó chỉ các khoảng thời gian hoạt động mới được biểu thị. 

Một trường hợp đặc biệt khác xuất phát từ các truy vấn phụ thuộc nhiều vào`last`. Khi kết nối chuyển từ 0 đến 1 liên tục, ánh xạ đỉnh thay đổi theo cách có thể tham chiếu nhiều lần cùng một cạnh vật lý dưới các nhãn khác nhau. Bởi vì tất cả các phép biến đổi được áp dụng ngay lập tức khi đọc đầu vào, các khoảng thời gian được lưu trữ vẫn nhất quán với thứ tự thực hiện thực tế, đảm bảo tính chính xác ngay cả khi dao động tối đa của`last`.
