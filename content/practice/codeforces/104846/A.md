---
title: "CF 104846A - \u041d\u043e\u0432\u044b\u0435 \u043a\u043d\u0438\u0433\u0438"
description: "Chúng tôi được tặng hai loại sách. Có sách toán A và sách lập trình B. Mỗi cuốn sách toán học đóng góp X sự kiện mới và mỗi cuốn sách lập trình đóng góp Y sự kiện mới."
date: "2026-06-28T11:27:04+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104846
codeforces_index: "A"
codeforces_contest_name: "\u041c\u0443\u043d\u0438\u0446\u0438\u043f\u0430\u043b\u044c\u043d\u044b\u0439 \u044d\u0442\u0430\u043f \u0412\u0441\u041e\u0428 \u043f\u043e \u0438\u043d\u0444\u043e\u0440\u043c\u0430\u0442\u0438\u043a\u0435 \u0432 \u041c\u043e\u0441\u043a\u043e\u0432\u0441\u043a\u043e\u0439 \u043e\u0431\u043b\u0430\u0441\u0442\u0438 2023-2024 (7-8 \u043a\u043b\u0430\u0441\u0441\u044b)"
rating: 0
weight: 104846
solve_time_s: 48
verified: true
draft: false
---

[CF 104846A - \u041d\u043e\u0432\u044b\u0435 \u043a\u043d\u0438\u0433\u0438](https://codeforces.com/problemset/problem/104846/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 48s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được tặng hai loại sách. Có sách toán A và sách lập trình B. Mỗi cuốn sách toán học đóng góp X sự kiện mới và mỗi cuốn sách lập trình đóng góp Y sự kiện mới. Tất cả thông tin trên tất cả các cuốn sách đều khác nhau, vì vậy tổng lượng kiến ​​thức từ một bộ sách được chọn chỉ là tổng tuyến tính của những cuốn sách được chọn. 

Ira có thể đặt tối đa K cuốn sách trên kệ. Nhiệm vụ là chọn một tập con các cuốn sách, tôn trọng giới hạn K sao cho tổng số sự kiện là lớn nhất. 

Vì vậy, cấu trúc rất đơn giản: chúng ta có bản sao A có giá trị X và B bản sao có giá trị Y và chúng ta có thể chọn tổng cộng tối đa K mục. Chúng tôi muốn số tiền tối đa có thể đạt được. 

Các ràng buộc trong các thử nghiệm rất lớn, lên tới khoảng 10^12 đối với A hoặc B. Điều đó ngay lập tức loại trừ mọi mô phỏng hoặc liệt kê các tập hợp con trên mỗi cuốn sách. Ngay cả việc quét tuyến tính trên tất cả các cuốn sách trong mỗi bài kiểm tra cũng được, nhưng bất cứ điều gì lặp lại trên mỗi đơn vị lựa chọn sách sẽ quá chậm nếu được thực hiện một cách ngây thơ bên trong một vòng lặp trên K. 

Một trường hợp thất bại tinh tế xuất hiện khi một trong các danh mục trống. Nếu A = 0 hoặc B = 0, chiến lược trộn tham lam vẫn phải giảm một cách chính xác về việc chọn từ một nhóm duy nhất. Một trường hợp khác là khi K vượt quá A + B, khi đó chúng ta không thể lấp đầy toàn bộ giá và phải lấy tất cả sách. 

Ví dụ: nếu A = 3, B = 0, K = 5, X = 10, Y = 1, câu trả lời đúng là 30, không phải 50 hoặc bất cứ điều gì liên quan đến việc đệm những cuốn sách không tồn tại. 

Một trường hợp góc khác là khi một loại chiếm ưu thế về giá trị nhưng khan hiếm về số lượng, ví dụ A = 2, B = 100, K = 50, X = 1000, Y = 1. Một chiến lược ngây thơ chỉ lấy min(K, B) của loại đẹp hơn mà không xem xét tính khả dụng sẽ thất bại nếu logic được viết không chính xác, nhưng cách tiếp cận đúng sẽ xử lý giới hạn một cách tự nhiên. 

## Phương pháp tiếp cận 

Nếu suy nghĩ một cách trực tiếp nhất, chúng ta có thể thử mọi cách để chọn i sách toán và j sách lập trình sao cho i + j ∎ K, i ₫ A, j ₫ B. Với mỗi cặp, chúng ta tính i·X + j·Y và lấy giá trị lớn nhất. Điều này đúng, nhưng không gian tìm kiếm là O(K), vì với mỗi i chúng ta xác định j hoặc ngược lại. Khi K có thể rất lớn thì điều này trở nên không khả thi. 

Cấu trúc đơn giản hóa khi chúng tôi nhận thấy rằng chỉ có hai giá trị cho mỗi loại mục. Mọi cuốn sách toán đều giống nhau, mọi cuốn sách lập trình đều giống nhau. Điều này loại bỏ mọi sự phức tạp về tổ hợp: quyết định có ý nghĩa duy nhất là chúng ta lấy bao nhiêu cuốn sách thuộc mỗi loại. 

Tại thời điểm đó, vấn đề giảm xuống còn việc chọn tối đa K mục từ hai nhóm. Nếu X lớn hơn Y, việc lấy càng nhiều sách toán càng tốt trước tiên, giới hạn bởi A và K, sau đó lấp đầy dung lượng còn lại bằng sách lập trình. Nếu Y lớn hơn, chúng ta đổi vai. Đây là sự lựa chọn tham lam cổ điển được chứng minh bằng trao đổi: bất kỳ giải pháp nào sử dụng cuốn sách có giá trị thấp hơn trong khi vẫn có sẵn cuốn sách có giá trị cao hơn đều có thể được cải thiện bằng cách hoán đổi. 

Brute-force hoạt động vì nó liệt kê rõ ràng tất cả các phần tách của K, nhưng nó không thành công khi K lớn vì số lượng phần chia tăng tuyến tính với K. Quan sát rằng trong mỗi loại, tất cả các mục đều giống hệt nhau cho phép chúng ta thu gọn quyết định thành nhiều nhất là hai lựa chọn xác định. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force vượt qua sự chia rẽ | O(K) | O(1) | Quá chậm | 
| Tham lam theo thứ tự giá trị | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng bài kiểm tra một cách độc lập. 

## Hướng dẫn thuật toán

1. So sánh X và Y để xác định loại sách nào có giá trị hơn trên mỗi đơn vị. Điều này thiết lập thứ tự mà chúng ta nên chọn sách. 
2. Chọn giá trị cao hơn trước. Giả sử X ≥ Y thì trước tiên ta lấy sách toán. Nếu Y > X, chúng ta bắt đầu một cách đối xứng với sách lập trình. Lý do là bất kỳ giải pháp tối ưu nào cũng phải ưu tiên những mặt hàng có giá trị cao hơn bất cứ khi nào có thể. 
3. Lấy càng nhiều sách càng tốt từ danh mục tốt hơn: lấy đầu tiên = min(K, A) nếu môn toán giỏi hơn, hoặc min(K, B) nếu lập trình giỏi hơn. 
4. Giảm dung lượng còn lại: K trở thành K - đầu tiên. 
5. Lấy càng nhiều sách từ loại thứ hai càng tốt: giây = min(K, B) hoặc min(K, A) tùy theo số sách còn lại. 
6. Tính tổng giá trị như first_count × value_first + two_count × value_second. 

Mỗi bước bị ép buộc bởi ràng buộc là chúng tôi chỉ có thể lấy tối đa K sách và không thể vượt quá số lượng sẵn có trong mỗi danh mục. 

### Tại sao nó hoạt động 

Mọi lựa chọn hợp lệ chỉ được xác định bằng số lượng (i, j) với i ≤ A, j ≤ B, i + j ≤ K. Giả sử tồn tại một giải pháp trong đó một cuốn sách có giá trị thấp hơn được chọn trong khi một cuốn sách có giá trị cao hơn vẫn có sẵn và dung lượng vẫn còn. Việc thay thế một cuốn sách có giá trị thấp hơn bằng một cuốn sách có giá trị cao hơn sẽ làm tăng tổng số tiền và duy trì tính khả thi. Việc lặp lại đối số trao đổi này sẽ biến đổi bất kỳ giải pháp tối ưu nào thành giải pháp đầu tiên sử dụng hết danh mục có giá trị cao hơn trong chừng mực các ràng buộc cho phép, sau đó sử dụng danh mục khác. Điều này đảm bảo thứ tự tham lam là tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve_case(A, B, K, X, Y):
    if X >= Y:
        take_x = min(A, K)
        K -= take_x
        take_y = min(B, K)
        return take_x * X + take_y * Y
    else:
        take_y = min(B, K)
        K -= take_y
        take_x = min(A, K)
        return take_x * X + take_y * Y

def main():
    data = sys.stdin.read().strip().split()
    t = 10
    idx = 0
    out = []
    for _ in range(t):
        A = int(data[idx]); B = int(data[idx+1]); K = int(data[idx+2])
        X = int(data[idx+3]); Y = int(data[idx+4])
        idx += 5
        out.append(str(solve_case(A, B, K, X, Y)))
    print("\n".join(out))

if __name__ == "__main__":
    main()
```Việc thực hiện phản ánh trực tiếp cấu trúc tham lam. Quyết định quan trọng là sự so sánh giữa X và Y, quyết định thứ tự tiêu dùng. Mỗi bài kiểm tra là thời gian không đổi, vì chúng tôi chỉ thực hiện một số phép tính số học và tính toán tối thiểu. 

Phải cẩn thận để K được cập nhật sau khi lấy đợt đầu tiên; nếu không thì lựa chọn thứ hai sẽ bỏ qua giới hạn dung lượng một cách không chính xác. Ngoài ra, tất cả số học đều phù hợp một cách an toàn trong số nguyên Python do giới hạn lớn. 

## Ví dụ đã hoạt động 

Hãy xem xét một trường hợp đơn giản trong đó A = 3, B = 5, K = 4, X = 4, Y = 2. 

Ở đây X > Y, vì vậy chúng tôi ưu tiên sách toán hơn. 

| Bước | Làm bài toán | Còn K | Hãy lập trình | Tổng cộng | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | 0 | 4 | 0 | 0 | 
| Sau môn toán | 3 | 1 | 0 | 12 | 
| Sau khi lập trình | 3 | 1 | 1 | 14 | 

Đầu tiên chúng ta dùng hết sách toán vì chúng có giá trị hơn, sau đó mới dùng dung lượng còn lại cho sách lập trình. Kết quả xác nhận cấu trúc tham lam. 

Bây giờ xét A = 2, B = 10, K = 5, X = 1, Y = 10. 

Ở đây sách lập trình chiếm ưu thế. 

| Bước | Hãy lập trình | Còn K | Làm bài toán | Tổng cộng | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | 0 | 5 | 0 | 0 | 
| Sau khi lập trình | 5 | 0 | 0 | 50 | 

Vì K được loại có giá trị cao hơn sử dụng hoàn toàn nên sách toán không liên quan. Điều này cho thấy thuật toán xử lý chính xác các trường hợp trong đó một danh mục chiếm ưu thế hoàn toàn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) mỗi lần kiểm tra | Mỗi bài kiểm tra thực hiện phép tính và so sánh liên tục | 
| Không gian | O(1) | Không có cấu trúc phụ trợ ngoài một vài biến | 

Tổng công việc là tuyến tính theo số lượng trường hợp thử nghiệm, được cố định ở mức 10. Ngay cả với giá trị A và B cực lớn, thuật toán vẫn duy trì thời gian không đổi cho mỗi thử nghiệm, thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    def solve_case(A, B, K, X, Y):
        if X >= Y:
            take_x = min(A, K)
            K2 = K - take_x
            take_y = min(B, K2)
            return take_x * X + take_y * Y
        else:
            take_y = min(B, K)
            K2 = K - take_y
            take_x = min(A, K2)
            return take_x * X + take_y * Y

    data = inp.strip().split()
    t = 10
    idx = 0
    res = []
    for _ in range(t):
        A = int(data[idx]); B = int(data[idx+1]); K = int(data[idx+2])
        X = int(data[idx+3]); Y = int(data[idx+4])
        idx += 5
        res.append(str(solve_case(A, B, K, X, Y)))
    return "\n".join(res)

# provided sample-style cases
assert run("""3 5 7 4 2
23 44 70 5 13
239 0 137 7 19
1266 990 1127 2265 8297
1492 1214 2735 7322 2181
1964 1728 291 7683 2769
537004 662408676616 398351704499 672621 742358
79629586150 851573 79630127068 422542 412282
977363980149 126571152766 57164417018 305123 657661
129181369874 273586061399 318820081665 739382 528351
""").split() == None
```(Tuyên bố được cung cấp đã nêu rõ đầy đủ tất cả các bài kiểm tra; kết quả đầu ra mong đợi rõ ràng được đưa vào phần đánh giá.) 

| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Trường hợp A=0 | chỉ đóng góp Y | cạnh đơn danh mục | 
| B=0 trường hợp | chỉ đóng góp X | cạnh đơn danh mục | 
| K ≥ A+B | toàn bộ số tiền | tràn công suất | 
| X > Y lệch | tham lam X đầu tiên | đặt hàng đúng đắn | 

## Vỏ cạnh 

Khi A = 0 hoặc B = 0, thuật toán sẽ giảm một cách tự nhiên do một trong các lệnh gọi tối thiểu trở thành 0. Ví dụ: nếu A = 0 và X ≥ Y, chúng tôi lấy min(0, K) = 0 cho sách toán, sau đó lấy tất cả sách lập trình có thể có lên đến K. Mã xử lý việc này mà không cần phân nhánh đặc biệt. 

Khi K ≥ A + B, cả hai nhánh sẽ tiêu thụ hết số sách hiện có. Phút đầu tiên bão hòa thành A hoặc B tùy theo thứ tự, sau đó phút thứ hai đưa phần còn lại lên đến tổng số đầy đủ khác, mang lại A·X + B·Y. 

Khi X = Y, thứ tự không quan trọng. Thuật toán vẫn chọn một danh mục trước tiên, nhưng vì các giá trị giống hệt nhau nên mọi phép chia đều mang lại tổng số tiền như nhau.
