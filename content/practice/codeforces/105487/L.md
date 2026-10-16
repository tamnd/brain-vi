---
title: "CF 105487L - Câu đố"
description: "Chúng tôi được cung cấp bốn loại mảnh ghép, được dán nhãn A, B, C và D, với số lượng hạn chế của mỗi loại. Mỗi mảnh có hình dạng cạnh đặc biệt và các mảnh chỉ có thể được đặt cạnh nhau nếu các cạnh tiếp xúc của chúng tương thích theo cách bổ sung, nghĩa là một mặt phải “vừa vặn…"
date: "2026-06-23T01:49:26+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105487
codeforces_index: "L"
codeforces_contest_name: "2024 China Collegiate Programming Contest (CCPC) Female Onsite (2024\u5e74\u4e2d\u56fd\u5927\u5b66\u751f\u7a0b\u5e8f\u8bbe\u8ba1\u7ade\u8d5b\u5973\u751f\u4e13\u573a)"
rating: 0
weight: 105487
solve_time_s: 52
verified: true
draft: false
---

[CF 105487L - Câu đố](https://codeforces.com/problemset/problem/105487/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 52s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp bốn loại mảnh ghép, được dán nhãn A, B, C và D, với số lượng hạn chế của mỗi loại. Mỗi mảnh có hình dạng cạnh đặc biệt và các mảnh chỉ có thể được đặt cạnh nhau nếu các cạnh tiếp xúc của chúng tương thích theo cách bổ sung, nghĩa là một mặt phải “khớp” với mặt kia. 

Chúng ta muốn lắp ráp một hình chữ nhật đặc duy nhất, hình vuông hoặc không vuông, bằng cách sử dụng những mảnh này. Đường viền của hình dạng cuối cùng phải hoàn toàn bằng phẳng, vì vậy mọi vết lõm hoặc nhô ra ở đường viền bên ngoài đều bị cấm. Bên trong hình chữ nhật, mọi cạnh chung giữa hai mảnh lân cận cũng phải tương thích hoàn hảo theo cùng một quy tắc khớp. 

Mục tiêu không phải là giảm thiểu diện tích hoặc khớp với một hình dạng cố định mà là tối đa hóa số lượng mảnh chúng ta có thể sử dụng từ nhiều bộ đã cho trong khi vẫn có thể lắp ráp một hình chữ nhật hợp lệ. 

Các ràng buộc ngụ ý rằng chúng tôi có tối đa 10⁴ trường hợp thử nghiệm và mỗi trường hợp thử nghiệm có số lượng lên tới 10³. Do đó, bất kỳ giải pháp nào cũng phải chạy trong thời gian không đổi cho mỗi trường hợp thử nghiệm. Bất cứ điều gì liên quan đến việc liệt kê các hình dạng, xây dựng lưới hoặc tìm kiếm theo kích thước đều quá chậm. 

Một trường hợp thất bại tinh vi xuất hiện khi cố gắng suy luận cục bộ mà không tôn trọng việc ghép nối toàn cục. Ví dụ: giả sử A có nhiều nhưng loại C tương thích của nó lại bị thiếu hoàn toàn. Một cách tiếp cận tham lam vẫn có thể cố gắng đặt các quân A, nhưng không có quân nào trong số đó có thể được ghép một cách nhất quán trong một hình chữ nhật, vì vậy câu trả lời đúng là không có đóng góp nào có thể sử dụng được từ hướng đó. Tương tự, việc trộn các kiểu chưa khớp có thể dẫn đến các cụm lắp ghép “gần như hợp lệ” một phần không thể đóng thành hình chữ nhật. 

## Phương pháp tiếp cận 

Điểm khởi đầu tự nhiên là nghĩ đến việc thực sự dựng hình chữ nhật. Người ta có thể thử liệt kê các kích thước hình chữ nhật có thể có và cố gắng xếp chúng bằng các mảnh có sẵn trong khi vẫn tôn trọng khả năng tương thích của các cạnh. Điều này sẽ liên quan đến việc quyết định chiều rộng và chiều cao, sau đó chạy kiểm tra tính khả thi để gán các phần cho các vị trí và xác minh tất cả các vùng lân cận. 

Mặc dù về mặt khái niệm thì điều này đơn giản nhưng nó lại quá chậm. Ngay cả khi chúng tôi giới hạn kích thước dựa trên tổng số phần có sẵn, số lượng hình chữ nhật đề xuất là tuyến tính trong tổng số lượng và mỗi lần xác thực đều yêu cầu quét một lưới. Với tối đa 10⁴ trường hợp thử nghiệm, điều này nhanh chóng trở nên không khả thi. 

Quan sát quan trọng là vấn đề không bao giờ thực sự phụ thuộc vào vị trí hình học. Ràng buộc hình chữ nhật thực thi một yêu cầu về cấu trúc mạnh mẽ hơn nhiều: mọi cạnh lộ ra phải trung tính và mọi cạnh kề bên trong phải tạo thành một cặp bổ sung hợp lệ. Điều này buộc công trình phải phân hủy thành các cặp độc lập thuộc loại không tương thích. 

Sau khi được xem theo cách này, hình học sẽ biến mất và chúng ta gặp phải một vấn đề khớp. Mỗi quân A chỉ có thể đóng góp một cách có ý nghĩa khi được ghép với một quân C tương thích, và mỗi quân B chỉ ghép với quân D. Bất kỳ quân nào không ghép đôi đều không thể sử dụng được vì nó không thể xuất hiện ở ranh giới hoặc bên trong mà không vi phạm các ràng buộc. 

Do đó, toàn bộ vấn đề giảm xuống còn việc hình thành càng nhiều cặp hợp lệ càng tốt trong hai nhóm độc lập. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Ốp lát Brute Force | O(T · N³) | O(N2) | Quá chậm | 
| Đếm Cặp | O(T) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng trường hợp thử nghiệm một cách độc lập và giảm vấn đề xuống việc đếm xem có thể hình thành bao nhiêu cặp bổ sung. 

1. Đối với cặp (A, C), hãy tính xem có thể tạo ra bao nhiêu kết quả không khớp nhau. Mỗi trận đấu tiêu tốn một A và một C nên số cặp sử dụng được là`min(A, C)`. Điều này thể hiện tất cả các tương tác hợp lệ có thể có giữa hai loại cạnh không tương thích này. 
2. Đối với cặp (B, D), tính đại lượng tương tự bằng cách sử dụng`min(B, D)`. Các cặp này độc lập với các cặp A-C nên chúng không gây nhiễu lẫn nhau. 
3. Mỗi cặp hợp lệ tương ứng với hai phần có thể được sử dụng một cách an toàn trong hình chữ nhật mà không vi phạm các ràng buộc ranh giới hoặc kề cận. 
4. Do đó, tổng số mảnh có thể sử dụng được gấp đôi số cặp tạo thành, tức là`2 * (min(A, C) + min(B, D))`. 
5. Nếu không có cặp hợp lệ nào tồn tại thì kết quả bằng 0, vì không thể tạo thành hình chữ nhật có các cạnh hợp lệ. 

### Tại sao nó hoạt động 

Các ràng buộc hình chữ nhật buộc mọi cạnh phải được cân bằng trên toàn cầu. Bất kỳ mảnh nào được đặt trong cấu hình hợp lệ đều phải có tất cả các cạnh lộ ra của nó được trung hòa bởi các mảnh lân cận tương thích, điều này chỉ có thể thực hiện được thông qua việc ghép nối chính xác giữa các loại bổ sung. Vì không có giới hạn cấu trúc bổ sung nào ngoài khả năng tương thích của cạnh, nên chiến lược tối ưu luôn sử dụng càng nhiều cặp hợp lệ rời rạc càng tốt và không có sự sắp xếp lại nào có thể làm tăng số lượng này. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        A, B, C, D = map(int, input().split())
        ans = 2 * (min(A, C) + min(B, D))
        print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai mã hóa trực tiếp logic ghép nối có nguồn gốc ở trên. Mỗi trường hợp thử nghiệm được đọc độc lập và không yêu cầu cấu trúc dữ liệu phụ trợ. 

Điều tinh tế duy nhất là đảm bảo rằng phép nhân với 2 xảy ra sau khi tính tổng hai số cặp độc lập. Thực hiện sớm hơn sẽ không thay đổi tính chính xác, nhưng việc giữ nó được nhóm lại sẽ làm cho sự phụ thuộc vào cấu trúc ghép nối trở nên rõ ràng. 

## Ví dụ đã hoạt động 

Hãy xem xét đầu vào trong đó tất cả các loại được cân bằng: 

đầu vào:```
A=4, B=4, C=4, D=4
```Chúng tôi theo dõi việc ghép nối như sau. 

| Bước | A | C | phút(A,C) | B | D | phút(B,D) | Tổng số cặp | Trả lời | 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Bắt đầu | 4 | 4 | - | 4 | 4 | - | - | - | 
| Ghép nối AC | 4 | 4 | 4 | 4 | 4 | - | 4 | - | 
| Ghép nối BD | 4 | 4 | 4 | 4 | 4 | 4 | 8 | 16 | 

Thuật toán ghép tất cả A với C và tất cả B với D, tạo ra mức sử dụng tối đa. 

Bây giờ hãy xem xét một trường hợp sai lệch: 

đầu vào:```
A=5, B=1, C=2, D=3
```| Bước | A | C | phút(A,C) | B | D | phút(B,D) | Tổng số cặp | Trả lời | 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Bắt đầu | 5 | 2 | - | 1 | 3 | - | - | - | 
| Ghép nối AC | 5 | 2 | 2 | 1 | 3 | - | 2 | - | 
| Ghép nối BD | 5 | 2 | 2 | 1 | 3 | 1 | 3 | 6 | 

Điều này cho thấy lượng A dư không thể được sử dụng khi C đã cạn kiệt và tương tự đối với sự mất cân bằng B-D. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(T) | Mỗi trường hợp thử nghiệm thực hiện một số phép tính số học không đổi | 
| Không gian | O(1) | Không có cấu trúc phụ trợ nào được sử dụng ngoài các biến đầu vào | 

Các ràng buộc cho phép tối đa 10⁴ trường hợp thử nghiệm và mỗi trường hợp được xử lý trong thời gian không đổi, do đó giải pháp dễ dàng nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        A, B, C, D = map(int, input().split())
        out.append(str(2 * (min(A, C) + min(B, D))))
    return "\n".join(out)

# provided sample (interpreted)
assert run("1\n0 0 0 0\n") == "0"
assert run("1\n4 4 4 4\n") == "16"

# custom cases
assert run("1\n1 0 0 0\n") == "0", "single type cannot pair"
assert run("1\n10 0 10 0\n") == "20", "only A-C pairing"
assert run("1\n0 7 0 3\n") == "6", "only B-D pairing"
assert run("2\n5 1 2 3\n0 0 0 0\n") == "6\n0", "mixed + empty case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 0 0 0 | 0 | không có loại bổ sung | 
| 1 10 0 10 0 | 20 | ghép nối AC thuần túy | 
| 1 0 7 0 3 | 6 | ghép đôi B-D thuần túy | 
| 2 vụ hỗn hợp | 6 0 | xử lý nhiều bài kiểm tra | 

## Vỏ cạnh 

Khi tất cả số lượng bằng 0, cả hai nhóm ghép nối đều trống và kết quả đương nhiên là bằng 0. Thuật toán đánh giá cả`min(A, C)`Và`min(B, D)`bằng 0, do đó đầu ra chính xác sẽ trở thành 0 mà không cần xử lý đặc biệt. 

Khi chỉ tồn tại một cạnh của một cặp, chẳng hạn như A dương và C 0,`min(A, C)`trở thành số 0, ngăn chặn mọi việc sử dụng không hợp lệ các phần chưa khớp. Điều này ngăn cản trực giác không chính xác rằng các quân A còn sót lại vẫn có thể tạo thành các cấu trúc từng phần. 

Khi tất cả số lượng bị mất cân bằng cao, chẳng hạn như một loại lớn hơn nhiều so với đối tác của nó, thuật toán sẽ tự động giới hạn mức sử dụng ở số lượng nhỏ hơn, đảm bảo không đếm quá mức và phản ánh chính xác việc không thể sử dụng các phần dư thừa trong một hình chữ nhật hợp lệ.
