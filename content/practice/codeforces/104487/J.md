---
title: "CF 104487J - Lazy Abdo"
description: "Chúng tôi được đưa ra một số kịch bản độc lập. Trong mỗi kịch bản, có một danh sách các nhiệm vụ và mỗi nhiệm vụ có thời lượng cố định tính bằng phút. Từ những nhiệm vụ này, chúng ta phải chọn chính xác k nhiệm vụ đó. Sau khi được chọn, thời lượng của chúng sẽ cộng lại và mục tiêu của chúng tôi là làm cho tổng thời lượng này lớn nhất có thể."
date: "2026-06-30T12:40:23+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104487
codeforces_index: "J"
codeforces_contest_name: "Tishreen + SVU CPC 2023"
rating: 0
weight: 104487
solve_time_s: 43
verified: true
draft: false
---

[CF 104487J - Cơ bụng lười biếng](https://codeforces.com/problemset/problem/104487/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 43s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được đưa ra một số kịch bản độc lập. Trong mỗi kịch bản, có một danh sách các nhiệm vụ và mỗi nhiệm vụ có thời lượng cố định tính bằng phút. Từ những nhiệm vụ này, chúng ta phải chọn chính xác k nhiệm vụ đó. Sau khi được chọn, thời lượng của chúng sẽ cộng lại và mục tiêu của chúng tôi là làm cho tổng thời lượng này lớn nhất có thể. 

Vì vậy, vấn đề giảm xuống còn việc chọn k số từ một mảng sao cho tổng của chúng là lớn nhất và chúng tôi xuất ra tổng tối đa có thể đó cho mỗi trường hợp thử nghiệm. 

Các ràng buộc nhỏ: n nhiều nhất là 100, giá trị nhiều nhất là 1000 và có nhiều nhất 100 trường hợp thử nghiệm. Điều này ngay lập tức loại trừ bất cứ điều gì nặng nề như tìm kiếm tổ hợp trên các tập hợp con có kích thước k theo cách trực tiếp, bởi vì điều đó sẽ liên quan đến việc chọn các kết hợp kích thước k trên n, tăng dần khi n chọn k. Ngay cả với n = 100, con số đó vẫn trở nên lớn về mặt thiên văn ở khoảng giữa của k, khiến cho việc sử dụng vũ lực là không thể thực hiện được. 

Một quan sát đơn giản nhưng quan trọng là thứ tự không quan trọng, chỉ có phần tử nào được chọn. Điều này cho thấy cấu trúc hoàn toàn chỉ nhằm chọn ra những người đóng góp lớn nhất. 

Các trường hợp đặc biệt đáng suy nghĩ là các tình huống trong đó tất cả các giá trị đều bằng nhau, trong đó k bằng n và k bằng 1. Ví dụ: nếu tất cả các nhiệm vụ giống hệt nhau thì bất kỳ lựa chọn nào cũng đưa ra cùng một câu trả lời, do đó kết quả đầu ra chỉ được k nhân với giá trị đó. Nếu k = n thì chúng ta phải lấy tất cả. Nếu k = 1 thì chúng ta phải lấy phần tử đơn lớn nhất. 

Một sai lầm ngây thơ sẽ là cố gắng mô phỏng tất cả các lựa chọn hoặc sử dụng chiến lược tham lam chọn k phần tử đầu tiên theo thứ tự đầu vào. Ví dụ: 

đầu vào: 

n = 5, k = 2 

a = [1, 100, 2, 3, 4] 

Lấy hai phần tử đầu tiên sẽ ra 1 + 100 = 101, nhưng đáp án đúng là 100 + 4 = 104. Điều này cho thấy vị trí trong mảng không liên quan. 

## Phương pháp tiếp cận 

Ý tưởng brute-force là thử mọi tập con có kích thước k, tính tổng của nó và lấy giá trị tối đa. Điều này đơn giản về mặt khái niệm: tạo ra các kết hợp, tính tổng từng kết hợp và theo dõi kết quả tốt nhất. Tính đúng đắn là hiển nhiên vì mọi lựa chọn hợp lệ đều được xem xét. 

Vấn đề là số lượng kết hợp. Số cách chọn k phần tử từ n là n chọn k, đạt giá trị lớn nhất xung quanh k = n/2. Với n = 100, giá trị này ở mức 10^29, vượt xa mọi tính toán khả thi trong giới hạn thời gian. Ngay cả khi tổng mỗi tập hợp con mất thời gian không đổi thì việc liệt kê một mình là không thể. 

Thông tin chi tiết quan trọng là vì chúng ta chỉ quan tâm đến việc tối đa hóa tổng nên chúng ta không bao giờ được hưởng lợi từ việc chọn giá trị nhỏ hơn khi có sẵn giá trị lớn hơn. Điều này có nghĩa là chiến lược tối ưu phải bao gồm việc chọn k phần tử lớn nhất trong mảng. Sau khi mảng được sắp xếp, câu trả lời chỉ đơn giản là tổng của k phần tử cuối cùng. 

Điều này làm giảm vấn đề từ lựa chọn tổ hợp đến sắp xếp và cắt lát. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (tất cả các tập con k) | O(2^n) hoặc O(n chọn k) | Ngăn xếp đệ quy O(k) | Quá chậm | 
| Sắp xếp và chọn top k | O(n log n) | O(1) hoặc O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng ca kiểm thử T, vì mỗi ca kiểm thử là độc lập và phải được xử lý riêng. 
2. Đối với mỗi trường hợp thử nghiệm, hãy đọc n và k, xác định số lượng nhiệm vụ tồn tại và số lượng chúng ta được phép chọn. 
3. Đọc mảng thời lượng nhiệm vụ. 
4. Sắp xếp mảng theo thứ tự không giảm. Việc sắp xếp là cần thiết vì nó nhóm các giá trị nhỏ nhất và lớn nhất theo cách khiến cho việc lựa chọn trở nên tầm thường. 
5. Lấy k phần tử cuối cùng của mảng đã sắp xếp, là k giá trị lớn nhất. 
6. Tính tổng của chúng và xuất ra. 

Mỗi bước được cấu trúc để loại bỏ sự không chắc chắn. Việc sắp xếp đảm bảo chúng ta có thể suy luận tổng thể về phần tử nào lớn nhất mà không cần phải tìm kiếm hoặc so sánh nhiều lần các tập hợp con. 

### Tại sao nó hoạt động

Bất kỳ lựa chọn hợp lệ nào gồm k phần tử đều có tổng bằng tổng của các phần tử đó. Nếu chúng tôi xem xét việc thay thế bất kỳ phần tử đã chọn nào bằng phần tử không được chọn lớn hơn thì tổng số tiền sẽ tăng lên một cách nghiêm ngặt. Việc lặp lại đối số thay thế này cho thấy rằng bất kỳ lựa chọn tối ưu nào cũng phải bao gồm toàn bộ k phần tử lớn nhất trong mảng. Vì việc sắp xếp các phần tử một cách rõ ràng theo kích thước nên hậu tố k trên cùng của mảng được sắp xếp chính xác là tập hợp đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    out = []
    for _ in range(t):
        n, k = map(int, input().split())
        a = list(map(int, input().split()))
        
        a.sort()
        out.append(str(sum(a[-k:])))
    
    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Việc thực hiện theo thuật toán trực tiếp. Việc sắp xếp được thực hiện cho mỗi trường hợp thử nghiệm vì mỗi mảng là độc lập. lát cắt`a[-k:]`trích xuất k phần tử lớn nhất sau khi sắp xếp. Tổng hợp chúng cho kết quả cuối cùng. 

Một chi tiết triển khai tinh tế là đảm bảo chúng tôi không vô tình sắp xếp theo thứ tự giảm dần và sau đó chọn sai phần. Cả hai cách tiếp cận đều có hiệu quả, nhưng việc trộn thứ tự và lập chỉ mục không chính xác là nguyên nhân phổ biến gây ra các lỗi riêng lẻ. Sử dụng sắp xếp tăng dần và lát cắt âm là mẫu ổn định nhất. 

## Ví dụ đã hoạt động 

Hãy xem xét đầu vào: 

n = 5, k = 3 

a = [1, 5, 3, 4, 2] 

Sau khi sắp xếp, ta được: 

[1, 2, 3, 4, 5] 

Ta lấy 3 phần tử cuối: [3, 4, 5] 

| Bước | Trạng thái mảng | Các yếu tố được chọn | Tổng hợp | 
| --- | --- | --- | --- | 
| Sau khi sắp xếp | [1, 2, 3, 4, 5] | - | - | 
| Lựa chọn | [1, 2, 3, 4, 5] | [3, 4, 5] | 12 | 

Điều này xác nhận thuật toán luôn chọn các giá trị lớn nhất. 

Bây giờ hãy xem xét ví dụ thứ hai: 

n = 4, k = 1 

a = [7, 13, 2, 5] 

Mảng được sắp xếp: 

[2, 5, 7, 13] 

Ta lấy 1 phần tử cuối cùng: [13] 

| Bước | Trạng thái mảng | Các yếu tố được chọn | Tổng hợp | 
| --- | --- | --- | --- | 
| Sau khi sắp xếp | [2, 5, 7, 13] | - | - | 
| Lựa chọn | [2, 5, 7, 13] | [13] | 13 | 

Điều này cho thấy trường hợp cực đoan khi chỉ có phần tử tối đa quan trọng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(T · n log n) | Mỗi trường hợp thử nghiệm sắp xếp tối đa 100 phần tử | 
| Không gian | O(n) | Lưu trữ mảng cho mỗi trường hợp thử nghiệm | 

Các ràng buộc cho phép tối đa 100 trường hợp thử nghiệm với n lên tới 100, do đó số phần tử được xử lý tối đa là 10.000. Việc sắp xếp từng mảng nhỏ dễ dàng nằm trong giới hạn và thời gian chạy tổng thể không đáng kể. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    import io as sio

    out = sio.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# sample-like cases
assert run("1\n5 3\n1 5 3 4 2\n") == "12"
assert run("1\n4 1\n7 13 2 5\n") == "13"

# all equal values
assert run("1\n5 3\n10 10 10 10 10\n") == "30"

# k equals n
assert run("1\n4 4\n1 2 3 4\n") == "10"

# k equals 1
assert run("1\n5 1\n9 1 8 2 7\n") == "9"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả đều bình đẳng | 30 | giá trị thống nhất hoạt động chính xác | 
| k = n | 10 | trường hợp cạnh lựa chọn đầy đủ | 
| k = 1 | 9 | lựa chọn tối đa duy nhất | 

## Vỏ cạnh 

Khi tất cả các giá trị giống hệt nhau, việc sắp xếp không thay đổi bất cứ điều gì và việc chọn bất kỳ phần tử k nào cũng mang lại tổng như nhau. Đối với đầu vào`[10, 10, 10, 10, 10]`với k = 3, việc sắp xếp sẽ cho cùng một mảng và ba phần tử cuối cùng có tổng bằng 30, khớp với bất kỳ lựa chọn hợp lệ nào. 

Khi k bằng n, thuật toán lấy toàn bộ mảng đã được sắp xếp. Vì`[1, 2, 3, 4]`, mảng được sắp xếp không thay đổi về thứ tự và tổng tất cả các phần tử sẽ bằng 10. Không có khả năng thiếu phần tử hoặc chọn sai vì lát cắt bao phủ toàn bộ phạm vi. 

Khi k bằng 1, thuật toán giảm xuống việc chọn phần tử lớn nhất. Vì`[9, 1, 8, 2, 7]`, sắp xếp cho`[1, 2, 7, 8, 9]`và lấy phần tử cuối cùng mang lại 9. Điều này phù hợp với định nghĩa tối đa hóa một lựa chọn duy nhất.
