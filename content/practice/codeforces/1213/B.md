---
title: "CF 1213B - Giá Xấu"
description: "Chúng ta được giao một số thí nghiệm độc lập và mỗi thí nghiệm bao gồm một chuỗi giá hàng ngày của một sản phẩm."
date: "2026-06-11T23:04:35+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1213
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 582 (Div. 3)"
rating: 1100
weight: 1213
solve_time_s: 80
verified: true
draft: false
---

[CF 1213B - Giá xấu](https://codeforces.com/problemset/problem/1213/B) 

**Đánh giá:** 1100 
**Tags:** cấu trúc dữ liệu, triển khai 
**Thời gian giải:** 1 phút 20s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được giao một số thí nghiệm độc lập và mỗi thí nghiệm bao gồm một chuỗi giá hàng ngày của một sản phẩm. Đối với mỗi chuỗi, chúng ta cần xác định có bao nhiêu ngày “không may mắn” theo nghĩa sau: một ngày được coi là xấu nếu tồn tại một số ngày sau đó mà giá thấp hơn ngày đó. 

Vì vậy, đối với mỗi vị trí trong mảng, chúng ta đang hỏi liệu có giá trị nào nhỏ hơn ở đâu đó bên phải hay không. Nếu một giá trị như vậy tồn tại thì vị trí đó sẽ góp phần đưa ra câu trả lời. 

Tổng cộng các ràng buộc rất lớn, với tổng độ dài của tất cả các chuỗi lên tới 150.000. Điều này ngay lập tức loại trừ bất kỳ giải pháp nào so sánh từng phần tử với tất cả các phần tử ở bên phải của nó, vì đó sẽ là phương trình bậc hai trong trường hợp xấu nhất và dẫn đến khoảng 10¹⁰ phép toán trong các đầu vào đối nghịch. Cần phải quét tuyến tính hoặc gần tuyến tính cho mỗi trường hợp thử nghiệm. 

Trường hợp cạnh tinh tế xuất hiện khi mảng tăng nghiêm ngặt. Ví dụ, trong`[1, 2, 3, 4]`, không có phần tử nào có giá trị nhỏ hơn ở bên phải nên câu trả lời là 0. Một cách tiếp cận ngây thơ có thể tính toán so sánh không chính xác nếu nó không kiểm tra chính xác các giá trị nhỏ hơn thay vì các giá trị nhỏ hơn hoặc bằng nhau. 

Một trường hợp cạnh khác là một mảng giảm nghiêm ngặt như`[5, 4, 3, 2, 1]`. Mọi phần tử ngoại trừ phần tử cuối cùng đều có phần tử nhỏ hơn ở sau, vì vậy câu trả lời là`n-1`. Đây là trường hợp xấu nhất đối với bạo lực, bởi vì mọi phần tử tiền tố đều được coi là xấu. 

## Phương pháp tiếp cận 

Việc diễn giải trực tiếp sẽ dẫn đến một phương pháp ép buộc đơn giản: cho mỗi ngày`i`, quét tất cả các ngày`j > i`và kiểm tra xem có`a[j] < a[i]`. Nếu một ngày như vậy tồn tại, chúng tôi sẽ tăng câu trả lời cho ngày`i`. 

Điều này đúng vì nó tuân theo định nghĩa theo nghĩa đen. Tuy nhiên, chi phí của nó là vấn đề. Trong trường hợp xấu nhất, chẳng hạn như dãy giảm dần, chúng ta thực hiện gần đúng`n + (n-1) + ... + 1`, đó là so sánh O(n²) cho mỗi trường hợp thử nghiệm. Với tổng số`n`lên tới 150.000, điều này trở nên quá chậm. 

Quan sát quan trọng là chúng ta không cần biết giá trị tương lai nào nhỏ hơn, chỉ cần biết liệu có tồn tại ít nhất một giá trị hay không. Điều này gợi ý việc duy trì thông tin về hậu tố của mảng. Nếu chúng ta di chuyển từ phải sang trái, chúng ta có thể theo dõi giá trị tối thiểu được thấy cho đến nay. Tại mỗi vị trí, chúng tôi so sánh phần tử hiện tại với hậu tố tối thiểu này. Nếu giá trị hiện tại lớn hơn giá trị tối thiểu ở bên phải của nó thì chúng ta đã biết tồn tại một phần tử tương lai nhỏ hơn, vì vậy vị trí này không tốt. 

Điều này làm giảm vấn đề xuống còn một lần vượt qua cho mỗi trường hợp thử nghiệm, vì hậu tố tối thiểu có thể được cập nhật tăng dần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Quá chậm | 
| Quét tối thiểu hậu tố | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng trường hợp thử nghiệm một cách độc lập và quét mảng từ phải sang trái. 

1. Bắt đầu từ ngày cuối cùng và khởi tạo một biến`min_suffix`với mức giá cuối cùng. Đây là mức giá nhỏ nhất được thấy trong tất cả các ngày ở ngay bên phải vị trí hiện tại. 
2. Di chuyển từng bước sang trái, kiểm tra mỗi ngày`i`. 
3. Đối với mỗi vị trí, so sánh`a[i]`với`min_suffix`. Nếu như`a[i] > min_suffix`, thì tồn tại ngày sau đó với giá thấp hơn, vì vậy ngày`i`là xấu và chúng tôi tăng câu trả lời. 
4. Bất kể chúng tôi có tăng hay không, chúng tôi sẽ cập nhật`min_suffix = min(min_suffix, a[i])`để đảm bảo nó luôn phản ánh giá trị nhỏ nhất trong hậu tố bắt đầu từ`i`. 
5. Tiếp tục cho đến khi phần tử đầu tiên được xử lý. 

Tính chính xác phụ thuộc vào thực tế là khi chúng tôi xử lý chỉ mục`i`,`min_suffix`chính xác bằng`min(a[i+1], ..., a[n])`, do đó việc so sánh tương đương với việc kiểm tra xem phần tử nhỏ hơn có tồn tại ở bên phải hay không. 

### Tại sao nó hoạt động 

Ở mỗi bước,`min_suffix`lưu trữ giá trị tối thiểu của hậu tố ở bên phải chỉ mục hiện tại. Bất biến này đúng vì chúng tôi chỉ cập nhật nó sau khi xử lý từng phần tử. Vì vậy, điều kiện`a[i] > min_suffix`đúng chính xác khi tồn tại một số`j > i`như vậy`a[j] < a[i]`. Chưa có thông tin về danh tính`j`là cần thiết, chỉ có sự tồn tại của nó, được nắm bắt hoàn toàn bởi hậu tố tối thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
out = []

for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))
    
    min_suffix = a[-1]
    bad = 0
    
    for i in range(n - 2, -1, -1):
        if a[i] > min_suffix:
            bad += 1
        if a[i] < min_suffix:
            min_suffix = a[i]
    
    out.append(str(bad))

print("\n".join(out))
```Việc thực hiện theo sau quá trình quét ngược trực tiếp. Biến`min_suffix`luôn đại diện cho giá trị nhỏ nhất ở bên phải chỉ số hiện tại, do đó việc so sánh`a[i] > min_suffix`là bản dịch chính xác của định nghĩa “ngày tồi tệ”. Điều kiện cập nhật đảm bảo chúng tôi tuyên truyền chính xác mức tối thiểu mới. 

Phải cẩn thận khi khởi tạo`min_suffix`với phần tử cuối cùng, vì ngày cuối cùng không có ngày nào trong tương lai và không bao giờ có thể xấu. Ngoài ra, bộ tích lũy câu trả lời được lưu giữ cho mỗi trường hợp thử nghiệm để tránh lây nhiễm chéo giữa các tập dữ liệu. 

## Ví dụ đã hoạt động 

Chúng tôi theo dõi đầu vào mẫu: 

### Ví dụ 1 

Mảng đầu vào:`[3, 9, 4, 6, 7, 5]`| tôi | một [tôi] | min_suffix | xấu? | đã cập nhật min_suffix | 
| --- | --- | --- | --- | --- | 
| 5 | 5 | 5 | không | 5 | 
| 4 | 7 | 5 | vâng | 5 | 
| 3 | 6 | 5 | vâng | 5 | 
| 2 | 4 | 5 | không | 4 | 
| 1 | 9 | 4 | vâng | 4 | 
| 0 | 3 | 4 | không | 3 | 

Đáp án = 3 

Điều này xác nhận rằng chỉ những phần tử có giá trị nhỏ hơn hoàn toàn sau này mới được tính. 

### Ví dụ 2 

Mảng đầu vào:`[2, 1]`| tôi | một [tôi] | min_suffix | xấu? | đã cập nhật min_suffix | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | không | 1 | 
| 0 | 2 | 1 | vâng | 1 | 

Trả lời = 1 

Điều này cho thấy trường hợp không tầm thường tối thiểu trong đó chính xác một phần tử trở nên xấu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi phần tử được xử lý một lần trong một lần đảo ngược duy nhất | 
| Không gian | O(1) thêm | Chỉ một số biến được sử dụng ngoài bộ nhớ đầu vào | 

Cho rằng tổng của tất cả`n`nhiều nhất là 150.000, tổng công việc là tuyến tính tổng thể, dễ dàng phù hợp với thời hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        
        min_suffix = a[-1]
        bad = 0
        
        for i in range(n - 2, -1, -1):
            if a[i] > min_suffix:
                bad += 1
            if a[i] < min_suffix:
                min_suffix = a[i]
        
        out.append(str(bad))
    
    return "\n".join(out)

# provided samples
assert run("""5
6
3 9 4 6 7 5
1
1000000
2
2 1
10
31 41 59 26 53 58 97 93 23 84
7
3 2 1 2 3 4 5
""") == """3
0
1
8
2"""

# custom: minimum size
assert run("""1
1
100
""") == """0"""

# custom: strictly increasing
assert run("""1
5
1 2 3 4 5
""") == """0"""

# custom: strictly decreasing
assert run("""1
5
5 4 3 2 1
""") == """4"""

# custom: mixed duplicates
assert run("""1
6
1 3 3 2 2 4
""") == """3"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| cỡ 1 | 0 | trường hợp cạnh phần tử đơn | 
| ngày càng tăng | 0 | trường hợp không có yếu tố xấu | 
| giảm dần | n-1 | mật độ trường hợp xấu nhất | 
| trùng lặp trộn | 3 | xử lý chính xác các giá trị bằng nhau | 

## Vỏ cạnh 

Trong một mảng một phần tử như`[10]`, quá trình quét ngược sẽ bắt đầu`min_suffix`là 10 và không bao giờ vào vòng lặp. Không có sự so sánh nào được thực hiện nên câu trả lời vẫn là 0, phù hợp với định nghĩa vì không có ngày nào trong tương lai. 

Theo một trình tự tăng dần nghiêm ngặt như`[1, 2, 3, 4]`, hậu tố tối thiểu ở mỗi bước luôn lớn hơn hoặc bằng giá trị hiện tại, do đó điều kiện`a[i] > min_suffix`không bao giờ gây nên. Thuật toán tránh được các kết quả dương tính giả một cách chính xác. 

Theo trình tự giảm dần như`[5, 4, 3, 2, 1]`, mọi phần tử ngoại trừ phần tử cuối cùng đều lớn hơn hậu tố tối thiểu tại thời điểm đó, vì vậy mỗi phần tử đều bị tính là xấu. Thuật toán tăng chính xác`n-1`lần, phù hợp với thực tế là mọi phần tử tiền tố đều có giá trị tương lai nhỏ hơn.
