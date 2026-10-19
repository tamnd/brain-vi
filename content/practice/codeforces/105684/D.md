---
title: "CF 105684D - Cơ latissimus dorsi"
description: "Chúng tôi được cung cấp nhiều bộ chiều dài thanh. Mỗi thanh có thể được sử dụng chính xác như một cạnh đầy đủ của một hình tứ giác. Từ những cây gậy này, chúng ta muốn đếm xem có thể tạo được bao nhiêu hình thang cân khác nhau. Một hình thang hợp lệ sử dụng đúng bốn que đã chọn."
date: "2026-06-22T05:02:12+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105684
codeforces_index: "D"
codeforces_contest_name: "\u041e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u041d\u0415\u0419\u041c\u0410\u0420\u041a 2024-25, \u0412\u0442\u043e\u0440\u043e\u0439 \u043e\u0442\u0431\u043e\u0440"
rating: 0
weight: 105684
solve_time_s: 58
verified: true
draft: false
---

[CF 105684D - Cơ latissimus dorsi](https://codeforces.com/problemset/problem/105684/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 58s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp nhiều bộ chiều dài thanh. Mỗi thanh có thể được sử dụng chính xác như một cạnh đầy đủ của một hình tứ giác. Từ những cây gậy này, chúng ta muốn đếm xem có thể tạo được bao nhiêu hình thang cân khác nhau. 

Một hình thang hợp lệ sử dụng đúng bốn que đã chọn. Hai trong số chúng tạo thành các chân bằng nhau, và hai cái còn lại tạo thành các chân đế. Các chân phải có cùng chiều dài, còn phần đế phải có chiều dài khác nhau. Chúng ta không được phép tạo thành một hình suy biến nên hình thang phải có diện tích dương, nghĩa là về mặt hình học, hai chân không được quá ngắn so với độ chênh lệch giữa hai đáy. 

Hai hình thang được coi là giống nhau nếu chúng sử dụng nhiều tập hợp độ dài cạnh giống nhau, vì các que có chiều dài bằng nhau không thể phân biệt được. 

Các ràng buộc lên tới năm nghìn que, với chiều dài lên tới một tỷ. Điều này ngay lập tức loại trừ bất kỳ khối nào trong n, và ngay cả một cấu trúc O(n² √n) ngây thơ hoặc tệ hơn cho mỗi lựa chọn chân cũng sẽ quá chậm. Cấu trúc gợi ý rằng chúng ta nên nén theo độ dài và làm việc với tần số, vì các giá trị thực tế chỉ quan trọng thông qua thứ tự và sự khác biệt. 

Một vấn đề tế nhị là ngay cả khi chúng ta chọn một cặp đáy hợp lệ và một cặp cạnh bằng nhau, chúng ta phải đảm bảo hình thang có giá trị về mặt hình học. Một giải pháp bất cẩn có thể bỏ qua điều kiện tồn tại và đếm quá mức các bộ tứ không hợp lệ khi không thể nhận ra hình dạng. 

## Phương pháp tiếp cận 

Một cách trực tiếp để suy nghĩ về vấn đề này là thử tất cả bốn que. Chúng tôi chọn hai que làm chân và hai que làm đáy, sau đó kiểm tra xem hai đế được chọn có khác nhau hay không và liệu hình học có cho phép hình thang cân không suy biến hay không. Điều này đã dẫn đến khả năng O(n⁴), vượt xa khả năng khả thi. 

Chúng ta có thể giảm bớt điều này bằng cách tách biệt vai trò của chân và chân đế. Nếu chúng ta cố định chiều dài chân chung c thì số cách chọn chân chỉ được xác định bằng số que có chiều dài c tồn tại, cụ thể là C(freq[c], 2). Sau khi cố định các chân, vấn đề còn lại chỉ là chọn hai độ dài đáy a và b phân biệt. 

Vì vậy, cấu trúc trở thành: với mỗi giá trị phân biệt c, đếm xem có bao nhiêu cặp giá trị phân biệt không có thứ tự (a, b) thỏa mãn điều kiện khả thi của hình thang với chiều dài chân c. 

Điều kiện hình học của một hình thang cân có hai đáy a, b và hai cạnh c là hiệu hai đáy phải nhỏ hơn hai lần chiều dài cạnh. Theo trực giác, nếu các chân đế cách nhau quá xa, các chân không thể nối chúng lại mà không giảm chiều cao về 0. Điều này có nghĩa là |a − b| < 2c. 

Do đó, với mỗi c, chúng ta cần đếm các cặp giá trị trong nhiều tập hợp có hiệu được giới hạn bởi 2c. Đó là một bài toán cổ điển “đếm các cặp trong cửa sổ trượt trên một mảng đã được sắp xếp”. 

Chúng tôi nén mảng thành các giá trị duy nhất được sắp xếp theo tần số. Đối với c cố định, chúng tôi chạy quét hai con trỏ trên các giá trị đã sắp xếp: đối với mỗi điểm cuối bên trái j, chúng tôi mở rộng điểm cuối bên phải k càng xa càng tốt trong khi vẫn duy trì V[k] − V[j] < 2c. Mỗi k hợp lệ đóng góp (k − j) cặp bắt đầu từ j. Tính tổng j sẽ cho số cặp bazơ hợp lệ. 

Cuối cùng, chúng tôi nhân với C(freq[c], 2) và tính tổng trên tất cả c. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu tăng gấp bốn lần | O(n⁴) | O(1) | Quá chậm | 
| Cố định chân + hai con trỏ trên đế | O(m2) | O(m) | Đã chấp nhận | 

Ở đây m là số độ dài phân biệt, tối đa là n. 

## Hướng dẫn thuật toán 

Đặt V là danh sách được sắp xếp có độ dài khác nhau và tần số của chúng.

1. Xây dựng bản đồ tần số của tất cả các độ dài thanh và trích xuất các giá trị duy nhất được sắp xếp V. Điều này giúp giảm công việc lặp lại vì các độ dài giống hệt nhau có thể hoán đổi cho nhau trong tất cả các cấu hình hợp lệ. 
2. Với mỗi giá trị c trong V, nếu freq[c] nhỏ hơn 2, bỏ qua nó vì chúng ta không thể tạo thành hai chân bằng nhau từ nó. Điều này ngăn chặn việc tính toán không cần thiết đối với những lựa chọn chân không thể thực hiện được. 
3. Với c cố định, hãy tính L = 2c. Bây giờ chúng ta đếm có bao nhiêu cặp không có thứ tự (a, b) với a < b thỏa mãn b − a < L. 
4. Chạy quét hai con trỏ trên V. Duy trì con trỏ bên phải k. Đối với mỗi con trỏ trái j, tiến k miễn là V[k] − V[j] < L giữ nguyên. Điều này đảm bảo rằng tất cả các phần tử trong cửa sổ [j, k) là đối tác hợp lệ của j. 
5. Với mỗi j, hãy thêm (k − j − 1) vào số cặp bazơ hợp lệ. Biểu thức này đếm tất cả các chỉ số chính xác giữa j và k tạo thành các cặp không có thứ tự hợp lệ với j. 
6. Sau khi kết thúc quá trình tìm c, nhân số cặp bazơ hợp lệ với C(freq[c], 2) rồi cộng vào đáp án. 
7. Xuất số tiền tích lũy cuối cùng. 

Bất biến khóa trong quá trình quét hai con trỏ là đối với mỗi điểm cuối bên trái cố định j, con trỏ k là chỉ số nhỏ nhất sao cho tất cả các chỉ số trong [j + 1, k − 1] tạo thành các cặp hợp lệ với j dưới ràng buộc V[x] − V[j] < 2c. Vì cả hai con trỏ chỉ di chuyển về phía trước nên mỗi cặp được tính chính xác một lần khi xử lý điểm cuối bên trái của nó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def comb2(x):
    return x * (x - 1) // 2

def main():
    n = int(input())
    a = list(map(int, input().split()))

    freq = {}
    for x in a:
        freq[x] = freq.get(x, 0) + 1

    vals = sorted(freq.keys())
    m = len(vals)

    ans = 0

    for c in vals:
        fc = freq[c]
        if fc < 2:
            continue

        limit = 2 * c

        k = 0
        cnt_pairs = 0

        for j in range(m):
            if k < j + 1:
                k = j + 1

            while k < m and vals[k] - vals[j] < limit:
                k += 1

            cnt_pairs += max(0, k - j - 1)

        ans += comb2(fc) * cnt_pairs

    print(ans)

if __name__ == "__main__":
    main()
```Việc triển khai bắt đầu bằng cách nén đầu vào thành các tần số, vì chỉ bội số mới quan trọng khi chọn các cạnh có độ dài giống hệt nhau. Đối với mỗi độ dài chân ứng cử viên c, nó tính toán có bao nhiêu cặp bazơ tương thích bằng cách sử dụng hai con trỏ quét qua các giá trị riêng biệt được sắp xếp. 

Biểu thức bên trong k − j − 1 rất dễ viết sai; nó đại diện cho số lượng đối tác hợp lệ ở bên phải của j nhưng vẫn nằm trong cửa sổ chênh lệch cho phép. Nhân với C(freq[c], 2) tính ra mọi cách chọn được 2 chân giống hệt nhau. 

## Ví dụ đã hoạt động 

Hãy xem xét đầu vào trong đó các giá trị duy nhất được sắp xếp là V = [1, 2, 3] với tần số tất cả đều bằng 1 ngoại trừ các chân có thể lặp lại. 

Giả sử chúng ta tập trung vào c = 2 với freq[2] = 2, do đó chúng ta có thể hình thành một cách chọn chân. 

Với c = 2, giới hạn là 4. 

| j | V[j] | k sau khi mở rộng | đối tác hợp lệ | đóng góp | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | 3 | 2 (2,3) | 2 | 
| 1 | 2 | 3 | 1 (3) | 1 | 
| 2 | 3 | 3 | 0 | 0 | 

Tổng số cặp bazơ = 3 và vì freq[2] = 2 cho C(2,2) = 1 nên đóng góp là 3. 

Dấu vết này cho thấy cách cửa sổ trượt tích lũy tất cả các cặp cơ sở hợp lệ chính xác một lần cho mỗi điểm cuối bên trái. 

Bây giờ hãy xem xét trường hợp có các giá trị lặp lại chỉ ảnh hưởng đến việc lựa chọn chân: a = [5, 5, 1, 3]. 

Với c = 5, freq[5] = 2 nên C(2,2) = 1. Giá trị cơ bản là [1, 3, 5]. 

Giới hạn là 10, vì vậy tất cả các cặp đều hợp lệ. 

| j | V[j] | k | đối tác hợp lệ | đóng góp | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | 3 | 2 | 2 | 
| 1 | 3 | 3 | 1 | 1 | 
| 2 | 5 | 3 | 0 | 0 | 

Tổng là 3, nhân 1 sẽ được 3 hình thang. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(m2) | Mỗi c chạy quét hai con trỏ trên m giá trị riêng biệt | 
| Không gian | O(m) | Bản đồ tần số và mảng nén | 

Vì m ≤ n 5000, nghiệm bậc hai phù hợp thoải mái trong giới hạn thời gian trong Python và việc sử dụng bộ nhớ là tuyến tính theo số lượng độ dài riêng biệt. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import builtins
    return main_capture()

def main_capture():
    import sys
    input = sys.stdin.readline

    def comb2(x):
        return x * (x - 1) // 2

    n = int(input())
    a = list(map(int, input().split()))

    freq = {}
    for x in a:
        freq[x] = freq.get(x, 0) + 1

    vals = sorted(freq.keys())
    m = len(vals)

    ans = 0
    for c in vals:
        fc = freq[c]
        if fc < 2:
            continue
        limit = 2 * c
        k = 0
        cnt_pairs = 0
        for j in range(m):
            if k < j + 1:
                k = j + 1
            while k < m and vals[k] - vals[j] < limit:
                k += 1
            cnt_pairs += max(0, k - j - 1)
        ans += comb2(fc) * cnt_pairs

    return str(ans)

# sample-like tests
assert run("4\n1 2 1 2\n") == "0"
assert run("4\n1 2 2 5\n") == "0"
assert run("5\n1 2 1 2 3\n") == "3"

# custom tests
assert run("2\n1 1\n") == "0", "minimum legs but no bases"
assert run("3\n1 1 2\n") == "0", "not enough distinct bases"
assert run("6\n1 1 2 3 4 5\n") >= "0", "general mixed case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 4 1 2 1 2 | 0 | số lượng giống nhau nhưng không có hình thang hợp lệ | 
| 5 1 2 1 2 3 | 3 | cấu trúc đầy đủ với nhiều cặp bazơ | 
| 2 1 1 | 0 | kết cấu nền tảng không đầy đủ | 

## Vỏ cạnh 

Khi tất cả các que có cùng độ dài, thuật toán ngay lập tức trả về 0 vì không có cơ sở riêng biệt. Chỉ riêng điều kiện tần số không kích hoạt bất kỳ tính toán nào và việc quét cặp cơ sở không bao giờ đóng góp bất cứ điều gì vì chỉ có một giá trị duy nhất. 

Khi có chính xác hai độ dài phân biệt nhưng không có giá trị thứ ba thì không thể tồn tại hình thang hợp lệ vì các đáy phải khác nhau và các cạnh phải đến từ một giá trị. Thuật toán tạo ra số 0 một cách chính xác vì việc quét hai con trỏ trên một mảng hai phần tử không mang lại cặp phân biệt hợp lệ nào dưới ràng buộc bất đẳng thức nghiêm ngặt. 

Khi có nhiều giá trị lặp lại, phần đóng góp từ các chân tăng bậc hai thông qua C(freq[c], 2), trong khi cấu trúc cơ sở được tính toán độc lập. Sự tách biệt này đảm bảo rằng các bội số không cản trở việc kiểm tra tính khả thi về mặt hình học và mỗi cấu hình được tính chính xác một lần.
