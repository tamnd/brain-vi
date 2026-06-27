---
title: "CF 105358F - Du Lịch"
description: "Chúng tôi đang theo dõi một giá trị đang phát triển duy nhất, đó là xếp hạng của người dùng. Xếp hạng bắt đầu ở giá trị ban đầu cố định là 1500 và sau đó thay đổi sau mỗi n cuộc thi sắp tới."
date: "2026-06-23T15:51:04+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105358
codeforces_index: "F"
codeforces_contest_name: "The 2024 ICPC Asia EC Regionals Online Contest (II)"
rating: 0
weight: 105358
solve_time_s: 48
verified: true
draft: false
---

[CF 105358F - Du lịch](https://codeforces.com/problemset/problem/105358/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 48s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang theo dõi một giá trị đang phát triển duy nhất, đó là xếp hạng của người dùng. Xếp hạng bắt đầu ở giá trị ban đầu cố định là 1500 và sau đó thay đổi sau mỗi n cuộc thi sắp tới. Đối với cuộc thi thứ i, thay đổi xếp hạng được đưa ra là ci và quy tắc cập nhật là phụ gia, nghĩa là xếp hạng mới trở thành xếp hạng trước đó cộng với ci. 

Nhiệm vụ không phải là tính toán xếp hạng cuối cùng mà là phát hiện thời điểm sớm nhất khi xếp hạng đang chạy lần đầu tiên đạt hoặc vượt quá 4000 sau khi áp dụng thay đổi của cuộc thi. Nếu xếp hạng không bao giờ đạt 4000 ở bất kỳ tiền tố nào, chúng tôi trả về -1. 

Do đó, cấu trúc này là một vấn đề tích lũy tiền tố với việc kiểm tra ngưỡng. Mỗi tổng tiền tố thể hiện xếp hạng sau số lượng cuộc thi đó, được dịch chuyển theo giá trị ban đầu 1500. 

Ràng buộc n lên tới 100000 ngụ ý rằng mọi giải pháp đều phải xử lý từng cuộc thi trong thời gian không đổi. Cách tiếp cận bậc hai liên tục tính lại tổng cho mỗi tiền tố sẽ yêu cầu khoảng 10^10 thao tác trong trường hợp xấu nhất, vượt xa giới hạn 1 giây trong Python. Điều này ngay lập tức thúc đẩy chúng tôi hướng tới chiến lược tích lũy một lần. 

Một trường hợp khó nhận thấy xuất phát từ những cập nhật tiêu cực. Vì ci có thể âm nên xếp hạng có thể dao động trên và dưới ngưỡng. Chúng tôi phải đảm bảo rằng chúng tôi trả về chỉ mục đầu tiên khi đạt đến ngưỡng chứ không phải chỉ mục cuối cùng. 

Một trường hợp đặc biệt khác là khi xếp hạng ban đầu đã cao hơn hoặc bằng 4000. Trong bài toán này, nó là 1500, do đó tình huống đó không xảy ra, nhưng một cách tiếp cận lý luận mạnh mẽ vẫn nên xử lý nó một cách tự nhiên như một kiểm tra tiền tố trước khi xử lý. 

## Phương pháp tiếp cận 

Ý tưởng trực tiếp nhất là mô phỏng quá trình theo đúng nghĩa đen. Chúng tôi duy trì xếp hạng hiện tại, bắt đầu từ 1500 và đối với mỗi cuộc thi, hãy tính lại xếp hạng bằng cách thêm ci. Sau mỗi lần cập nhật, chúng tôi kiểm tra xem xếp hạng có ít nhất là 4000 hay không. Nếu vậy, chúng tôi sẽ trả về chỉ mục hiện tại. 

Cách tiếp cận này đúng vì nó phản ánh chính xác quá trình. Tuy nhiên, nó cũng đã tối ưu về cấu trúc. Sự kém hiệu quả duy nhất sẽ đến từ việc tính toán lại các tổng tiền tố nhiều lần nếu chúng ta thử một công thức đơn giản như kiểm tra mọi tiền tố bằng cách tính tổng lại từ đầu. Biến thể đó sẽ yêu cầu các phép toán O(n^2) vì mỗi tổng tiền tố sẽ có giá O(n) và có n tiền tố. 

Quan sát quan trọng là việc cập nhật xếp hạng hoàn toàn mang tính tích lũy. Không có phân nhánh, không đặt lại và không phụ thuộc vào giá trị tương lai. Điều này có nghĩa là trạng thái ở bước i hoàn toàn được xác định bởi trạng thái ở bước i - 1, vì vậy chúng ta có thể duy trì tổng số đang chạy thay vì tính toán lại bất kỳ thứ gì. 

Chúng tôi giảm vấn đề xuống còn một lần quét với tổng chạy, kiểm tra ngưỡng sau mỗi lần cập nhật. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tính lại tổng tiền tố từ đầu | O(n^2) | O(1) | Quá chậm | 
| Chạy tổng tích lũy | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì một biến đại diện cho xếp hạng hiện tại và cập nhật nó một cách tuần tự. 

1. Khởi tạo xếp hạng thành 1500, vì đây là trạng thái bắt đầu trước khi áp dụng bất kỳ cuộc thi nào. 
2. Lặp lại các cuộc thi từ i = 1 đến n theo thứ tự, đọc từng ci và thêm nó vào xếp hạng hiện tại. Bước này thể hiện việc áp dụng sự thay đổi dự đoán của cuộc thi thứ i. 
3. Sau khi cập nhật xếp hạng ở chỉ mục i, hãy kiểm tra xem xếp hạng có ít nhất là 4000 hay không. Nếu có, ngay lập tức trả về i ngay khi đạt đến ngưỡng đầu tiên. Việc thoát sớm là rất quan trọng vì chúng tôi muốn xảy ra lần đầu tiên. 
4. Nếu vòng lặp kết thúc mà rating chưa đạt tới 4000, hãy trả về -1. 

Lựa chọn thiết kế quan trọng là thực hiện kiểm tra ngưỡng ngay sau mỗi lần cập nhật thay vì trì hoãn nó. Điều đó đảm bảo chúng tôi nắm bắt được chỉ mục sớm nhất thay vì chỉ mục muộn hơn khi điều kiện vẫn được giữ nguyên. 

### Tại sao nó hoạt động

Thuật toán duy trì bất biến rằng sau khi xử lý phần tử i, xếp hạng được lưu trữ bằng chính xác 1500 cộng với tổng của c1 đến ci. Điều này diễn ra trực tiếp từ quy tắc cập nhật được áp dụng tuần tự. 

Bởi vì mỗi bước chỉ thêm ci một lần và không bao giờ xem lại các giá trị trong quá khứ nên không có trạng thái tiền tố nào có thể bị bỏ qua hoặc thay đổi sau này. Do đó, khi xếp hạng lần đầu tiên vượt qua hoặc đạt 4000 ở chỉ số i nào đó, thuật toán sẽ phát hiện nó ngay lập tức ở bước đó. Vì chúng tôi quay lại ngay lập tức nên không chỉ mục nào sau này có thể ghi đè câu trả lời này, đảm bảo tính tối thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    arr = list(map(int, input().split()))
    
    rating = 1500
    
    for i, c in enumerate(arr, 1):
        rating += c
        if rating >= 4000:
            print(i)
            return
    
    print(-1)

if __name__ == "__main__":
    solve()
```Giải pháp giữ một bộ tích lũy số nguyên duy nhất được gọi là xếp hạng. Nó bắt đầu từ năm 1500 và được cập nhật tại chỗ. Bảng liệt kê được lập chỉ mục 1 để khớp với cách đánh số trực tiếp của cuộc thi, tránh nhầm lẫn. 

Việc kiểm tra có điều kiện được thực hiện ngay sau mỗi lần cập nhật. Việc quay lại ngay lập tức đảm bảo chúng tôi không vô tình báo cáo chỉ mục hợp lệ sau này. 

Không có cấu trúc dữ liệu nào ngoài mảng đầu vào nên mức sử dụng bộ nhớ vẫn ở mức tối thiểu. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
1000 1000 1000 -5000 1000
```Chúng tôi mô phỏng từng bước. 

| tôi | ci | đánh giá trước | đánh giá sau | >= 4000 | 
| --- | --- | --- | --- | --- | 
| 1 | 1000 | 1500 | 2500 | Không | 
| 2 | 1000 | 2500 | 3500 | Không | 
| 3 | 1000 | 3500 | 4500 | Có | 

Tại i = 3, xếp hạng trở thành 4500, vượt qua ngưỡng, vì vậy chúng tôi xuất ra 3 ngay lập tức. 

Dấu vết này cho thấy các cập nhật phủ định sau này không thành vấn đề vì chúng tôi dừng ở tiền tố hợp lệ đầu tiên. 

### Ví dụ 2 

đầu vào:```
4
-100 -200 50 -300
```| tôi | ci | đánh giá trước | đánh giá sau | >= 4000 | 
| --- | --- | --- | --- | --- | 
| 1 | -100 | 1500 | 1400 | Không | 
| 2 | -200 | 1400 | 1200 | Không | 
| 3 | 50 | 1200 | 1250 | Không | 
| 4 | -300 | 1250 | 950 | Không | 

Không có tiền tố nào đạt tới 4000 nên đầu ra là -1. 

Điều này xác nhận thuật toán xử lý chính xác các trường hợp xếp hạng giảm đơn điệu hoặc không bao giờ đạt đến ngưỡng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi cuộc thi được xử lý đúng một lần với cập nhật và kiểm tra O(1) | 
| Không gian | O(1) | Chỉ một số nguyên đang chạy duy nhất được lưu trữ bên cạnh đầu vào | 

Quét tuyến tính là tối ưu vì mọi giá trị đầu vào phải được đọc ít nhất một lần. Với n lên tới 100000, điều này thoải mái phù hợp với giới hạn thời gian thông thường đối với Python. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

def solve():
    import sys
    input = sys.stdin.readline
    n = int(input())
    arr = list(map(int, input().split()))
    rating = 1500
    for i, c in enumerate(arr, 1):
        rating += c
        if rating >= 4000:
            print(i)
            return
    print(-1)

# provided sample
assert run("5\n1000 1000 1000 -5000 1000\n") == "3"

# minimum size, no reach
assert run("1\n100\n") == "-1"

# immediate reach
assert run("1\n3000\n") == "1"

# boundary just below threshold
assert run("2\n1000 1499\n") == "-1"

# crossing exactly at boundary
assert run("3\n1000 1000 500\n") == "3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 5 trình tự vượt sớm | 3 | chấm dứt sớm đúng đắn | 
| giá trị nhỏ duy nhất | -1 | không có kết quả dương tính giả | 
| bước nhảy lớn duy nhất | 1 | phát hiện ngay lập tức | 
| gần ngưỡng nhưng chưa đủ | -1 | an toàn từng người một | 
| vượt ngưỡng chính xác sau | 3 | hành vi tích lũy đúng đắn | 

## Vỏ cạnh 

Trường hợp cạnh chính là khi xếp hạng vượt qua ngưỡng chính xác ở tiền tố và sau đó giảm xuống. Ví dụ: 

đầu vào:```
4
1000 1000 -500 2000
```Từng bước một: 

Sau 1: 2500 

Sau 2: 3500 

Sau 3: 3000 

Sau 4: 5000 

Mặc dù xếp hạng giảm xuống dưới 4000 ở bước 3 nhưng câu trả lời đúng vẫn là 4 vì chúng tôi chỉ quan tâm đến lần đầu tiên xếp hạng đạt hoặc vượt 4000, điều này xảy ra ở bước 4. 

Thuật toán xử lý việc này một cách tự nhiên vì nó chỉ kiểm tra điều kiện sau mỗi lần cập nhật và trả về ngay lập tức khi thành công lần đầu tiên. Không có thành công nào trước đó bị ghi đè, vì vậy khi tìm thấy chỉ mục hợp lệ, các dao động sau đó sẽ không còn liên quan.
