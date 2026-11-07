---
problem: 1363F
contest_id: 1363
problem_index: F
name: "Rotating Substrings"
contest_name: "Codeforces Round 646 (Div. 2)"
rating: 2600
tags: ["dp", "strings"]
answer: passed_samples
verified: false
solve_time_s: 92
date: 2026-06-14
model: gpt-5-3-mini
samples_passed: 0
samples_total: 0
---

#CF 1363F - Chuỗi con xoay 

**Đánh giá:** 2600 
**Thẻ:** dp, chuỗi 
**Mẫu:** gpt-5-3-mini 
**Thời gian giải:** 1m 32s 
**Đã xác minh:** không (0/0 mẫu) 

--- 

##Giải pháp 

## Hiểu vấn đề 

Chúng ta được cho hai chuỗi có độ dài bằng nhau. Nhiệm vụ là chuyển đổi chuỗi đầu tiên thành chuỗi thứ hai bằng cách sử dụng một thao tác được phép duy nhất được áp dụng lặp đi lặp lại. Thao tác không hoán đổi các ký tự liền kề hoặc phân đoạn đảo ngược. Thay vào đó, nó lấy bất kỳ chuỗi con liền kề nào đã chọn và thực hiện xoay vòng theo chu kỳ bằng cách di chuyển ký tự cuối cùng của nó lên phía trước trong khi dịch chuyển mọi thứ khác sang một vị trí. 

Vì vậy, mỗi bước di chuyển có thể được coi là lấy một khối và xoay nó bên trong một bước. Các ký tự bên ngoài khối được chọn vẫn cố định tại chỗ. Mục tiêu là tìm ra số phép quay tối thiểu cần thiết để biến chuỗi ban đầu thành chuỗi mục tiêu hoặc kết luận rằng không có chuỗi phép quay nào có thể đạt được phép biến đổi. 

Các ràng buộc đủ chặt chẽ để giải pháp bậc hai hoặc bậc ba cho mỗi bài kiểm tra chỉ được chấp nhận với tổng độ dài lên tới hai nghìn. Điều này ngay lập tức loại trừ bất kỳ cách tiếp cận nào cố gắng mô phỏng tất cả các hoạt động chuỗi con hoặc xây dựng tìm kiếm theo cấp số nhân trên các cấu hình. Giải pháp phải suy luận về vị trí của các ký tự hơn là mô phỏng các phép biến đổi. 

Một điểm tinh tế quan trọng là phép quay bảo toàn nhiều bộ ký tự, nhưng điều đó là chưa đủ. Quan trọng hơn, thao tác này chỉ di chuyển một ký tự từ phần cuối của phân đoạn đã chọn đến phần đầu của nó, nghĩa là nó hoạt động giống như chuyển động của bong bóng được điều khiển bên trong một cửa sổ. Hạn chế này khiến không thể tự ý sắp xếp lại các ký tự trên các vùng rời rạc mà không phải trả tiền cho cấu trúc trung gian. 

Một lỗi phổ biến là cho rằng vì chúng ta có thể xoay bất kỳ chuỗi con nào nên chúng ta có thể tạo ra bất kỳ hoán vị ký tự nào miễn là tần số khớp. Điều này là sai. Ví dụ, chuyển đổi`ab`ĐẾN`ba`là có thể, nhưng chuyển đổi`abc`ĐẾN`bca`có thể thực hiện được trong một lần di chuyển, trong khi một số cách sắp xếp lại để bảo toàn số lượng vẫn yêu cầu nhiều bước có cấu trúc. Hoạt động luôn duy trì trật tự tương đối bên ngoài phân đoạn đã chọn, điều này tạo ra các ràng buộc ẩn. 

Một trường hợp cạnh khác là khi cả hai chuỗi giống hệt nhau. Câu trả lời là 0 và bất kỳ nỗ lực tham lam nào luôn thực hiện ít nhất một thao tác sẽ bị tính quá mức một cách không chính xác. 

Cuối cùng, những trường hợp không thể xảy ra khi các chuỗi không phải là phép đảo chữ. Không có chuỗi xoay nào có thể thay đổi được số ký tự nên những trường hợp như vậy phải quay lại ngay`-1`. 

## Phương pháp tiếp cận 

Cách giải thích brute-force là coi mỗi chuỗi là một trạng thái và mỗi phép quay chuỗi con là một cạnh, sau đó tìm kiếm đường đi ngắn nhất từ ​​`s`ĐẾN`t`. Từ bất kỳ trạng thái nào, đều có các chuỗi con O(n²) và mỗi lần di chuyển sẽ tạo ra một cấu hình mới. Không gian trạng thái là n giai thừa, vì chúng ta đang hoán vị các ký tự. Ngay cả việc tạo ra hàng xóm cũng trở nên tốn kém và BFS hoàn toàn không khả thi. 

Quan sát quan trọng là chúng ta không cần khám phá các cấu hình trên toàn cầu. Thay vào đó, chúng ta có thể xử lý chuỗi đích từ trái sang phải và cố gắng “cố định” chuỗi hiện tại vào đúng vị trí. Tại mỗi vị trí, chúng tôi quyết định xem có cần đưa một nhân vật cụ thể về phía trước hay không và nếu có, chúng tôi sẽ đo khoảng cách của nó và cần bao nhiêu phép quay có cấu trúc để dịch chuyển nhân vật đó vào vị trí. 

Thông tin chi tiết về cấu trúc quan trọng là việc xoay trên chuỗi con tương đương với việc lấy một ký tự từ vị trí r và chèn nó vào vị trí l. Đây thực sự là một hoạt động “tiến lên phía trước trong một phân khúc”. Sau khi được xem theo cách này, vấn đề sẽ trở thành việc liên tục trích xuất ký tự được yêu cầu tiếp theo và đếm xem nó nhảy qua bao nhiêu ký tự theo cách được kiểm soát. 

Thay vì mô phỏng các chuỗi đầy đủ, chúng tôi theo dõi vị trí hiện tại của các ký tự và dần dần khớp với mục tiêu, đếm số lần đảo ngược phải được giải quyết cục bộ. Mỗi sự không khớp có thể được giải quyết một cách tối ưu bằng cách đưa ký tự cần thiết từ vị trí hợp lệ gần nhất và chi phí được xác định bằng số lượng ký tự không khớp nằm giữa. 

Giải pháp giảm thiểu việc quét từ trái sang phải trong khi vẫn duy trì cấu trúc cho chúng ta biết vị trí xuất hiện tiếp theo của mỗi ký tự và bao nhiêu vị trí đã được sử dụng nằm trước ký tự đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force BFS trên dây | O(n! · n²) | O(n! · n) | Quá chậm | 
| Theo dõi vị trí + khớp tham lam | O(n²) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng trường hợp thử nghiệm một cách độc lập. 

1. Trước tiên hãy kiểm tra xem hai chuỗi có chứa nhiều bộ ký tự giống nhau hay không. Nếu không, hãy trả về -1 ngay lập tức vì không có thao tác nào thay đổi số lượng ký tự. 
2. Xây dựng danh sách vị trí cho từng ký tự trong chuỗi nguồn. Điều này cho phép chúng tôi nhanh chóng xác định vị trí hiện tại của mỗi ký tự được yêu cầu. 
3. Duy trì một con trỏ trên chuỗi đích. Đối với mỗi vị trí mục tiêu i, chúng ta cần ký tự thứ i xuất hiện ở vị trí i trong chuỗi làm việc. 
4. Với mỗi vị trí i, hãy tìm
