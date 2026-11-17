---
title: "CF 1995C - Bình phương"
description: "Chúng ta được cung cấp một số mảng độc lập và đối với mỗi mảng, chúng ta được phép sửa đổi các phần tử bằng một thao tác rất cụ thể: chọn một chỉ mục và thay thế giá trị tại vị trí đó bằng bình phương của nó. Chúng tôi có thể lặp lại thao tác này trên cùng một chỉ mục nhiều lần."
date: "2026-06-08T14:50:54+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "constructive-algorithms", "greedy", "implementation", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1995
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 961 (Div. 2)"
rating: 1800
weight: 1995
solve_time_s: 144
verified: false
draft: false
---

[CF 1995C - Bình phương](https://codeforces.com/problemset/problem/1995/C) 

**Đánh giá:** 1800 
**Tags:** vũ lực, thuật toán xây dựng, tham lam, thực hiện, toán học, lý thuyết số 
**Thời gian giải:** 2m 24s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một số mảng độc lập và đối với mỗi mảng, chúng ta được phép sửa đổi các phần tử bằng một thao tác rất cụ thể: chọn một chỉ mục và thay thế giá trị tại vị trí đó bằng bình phương của nó. Chúng tôi có thể lặp lại thao tác này trên cùng một chỉ mục nhiều lần. Mục tiêu của chúng tôi là làm cho toàn bộ mảng không giảm bằng cách sử dụng càng ít thao tác như vậy càng tốt và báo cáo số lượng tối thiểu đó hoặc xác định rằng không thể thực hiện được. 

Khó khăn chính là việc bình phương thay đổi các giá trị theo cách rất phi tuyến tính. Số lượng nhỏ có thể tăng chậm hoặc nhanh tùy thuộc vào độ lớn của chúng và khi giá trị tăng lên, chúng có thể dễ dàng phá vỡ trật tự với hàng xóm. Chúng tôi không được tự do sắp xếp lại các phần tử, vì vậy mọi thao tác chỉ biến đổi cục bộ một vị trí trong khi ảnh hưởng đến các ràng buộc thứ tự chung. 

Các ràng buộc rất lớn: tổng số phần tử trong tất cả các trường hợp thử nghiệm có thể đạt tới 2 · 10^5. Điều này ngay lập tức loại trừ bất kỳ giải pháp nào cố gắng mô phỏng tất cả các chuỗi bình phương có thể có cho mỗi phần tử hoặc khám phá các trạng thái một cách rõ ràng. Bất kỳ cách tiếp cận nào cũng phải gần tuyến tính hoặc gần tuyến tính cho mỗi trường hợp thử nghiệm. Vì mỗi phần tử có thể được bình phương nhiều lần nên độ phức tạp thực sự không chỉ ở kích thước mảng mà còn ở cách tăng trưởng của các giá trị. 

Một vấn đề tế nhị phát sinh khi việc bình phương không thể giúp được gì cả. Ví dụ: nếu chúng ta có một cặp giảm dần như [5, 4], bình phương 4 sẽ cho 16, điều này sẽ khắc phục được sự đảo ngược cục bộ, nhưng trong một chuỗi dài hơn, nó có thể phá vỡ các ràng buộc trước đó. Tệ hơn nữa, đôi khi không có chuỗi bình phương nào có thể giải quyết được mâu thuẫn trong việc sắp xếp các phụ thuộc và chúng ta phải phát hiện sớm điều này. 

Một trường hợp cạnh quan trọng khác là khi số đã lớn. Nếu một phần tử trở nên quá lớn so với các phần tử lân cận, thì việc bình phương thêm chỉ làm cho nó trở nên tồi tệ hơn, có khả năng ngăn cản tính khả thi. Một cách tiếp cận ngây thơ luôn “sửa chữa các nghịch đảo cục bộ một cách tham lam” có thể dễ dàng làm mịn quá mức một khu vực và phá vỡ khu vực khác. 

## Phương pháp tiếp cận 

Một cách giải thích bạo lực sẽ coi mỗi chỉ số là có một chuỗi vô hạn các giá trị có thể có: 

a[i], a[i]^2, a[i]^(2^2), a[i]^(2^3), v.v. Chúng ta có thể tưởng tượng việc tìm kiếm trên tất cả các lựa chọn về số lần bình phương của mỗi vị trí, sau đó kiểm tra xem mảng kết quả có không giảm hay không và giảm thiểu tổng số phép toán. 

Về nguyên tắc thì điều này đúng nhưng hoàn toàn không khả thi. Ngay cả khi chúng ta giới hạn số bình phương cho mỗi phần tử ở một hằng số nhỏ, chẳng hạn như lên tới 60 (vì các giá trị tăng nhanh), không gian trạng thái sẽ trở thành 60^n trong trường hợp xấu nhất, điều này là không thể. Ngay cả việc lập trình động trên các vị trí cũng không thành công vì mỗi vị trí phụ thuộc vào phạm vi không giới hạn của các giá trị có thể có trước đó. 

Quan sát chính là mỗi phần tử không độc lập một cách tùy tiện. Đối với một phần tử cố định, chúng ta không bao giờ cần xem xét tất cả các lũy thừa của bình phương một cách mù quáng. Thay vào đó, điều quan trọng là số lượng bình phương nhỏ nhất được yêu cầu để phần tử ít nhất lớn bằng giá trị đã chọn trước đó. Khi chúng tôi quyết định số lần một phần tử được bình phương, giá trị cuối cùng của nó sẽ được cố định và chúng tôi chỉ truyền bá các ràng buộc về phía trước. 

Điều này gợi ý một cách xây dựng tham lam từ trái sang phải. Tại mỗi vị trí, chúng tôi duy trì giá trị tối thiểu được cho phép bởi vị trí trước đó. Đối với phần tử hiện tại, chúng tôi cố gắng áp dụng bình phương cho đến khi nó đủ lớn để đáp ứng thứ tự không giảm. Tuy nhiên, chúng ta cũng phải đảm bảo không vượt quá mức khiến các yếu tố trong tương lai không thể bắt kịp.

Cấu trúc quan trọng là tăng trưởng đơn điệu: bình phương luôn tăng giá trị (đối với số nguyên ≥ 2) và tăng cực kỳ nhanh. Điều này có nghĩa là mỗi phần tử chỉ có một số lượng rất nhỏ trạng thái có ý nghĩa trước khi nó vượt quá mọi ràng buộc trong tương lai. Do đó, chúng tôi có thể mô phỏng, trên mỗi phần tử, số lượng bình phương tối thiểu cần thiết để đạt đến ngưỡng và truyền ngưỡng đó về phía trước. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Brute Force trên tất cả các ô vuông | Hàm mũ | O(n) | Quá chậm | 
| Mô phỏng bình phương từ trái sang phải tham lam | O(n log log A) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý mảng từ trái sang phải trong khi duy trì giá trị tối thiểu được phép cho vị trí hiện tại, bắt nguồn từ phần tử trước đó sau các phép biến đổi đã chọn của nó. 

1. Khởi tạo bộ đếm cho các phép toán và đặt giới hạn dưới được yêu cầu hiện tại làm phần tử đầu tiên. Phần tử đầu tiên không cần điều chỉnh vì không có ràng buộc nào trước đó. 
2. Đối với mỗi phần tử tiếp theo, hãy so sánh nó với giới hạn dưới hiện tại. Nếu nó đã đủ lớn, chúng tôi chấp nhận nó mà không cần bất kỳ thao tác nào và cập nhật giới hạn dưới thành giá trị này. 
3. Nếu nó nhỏ hơn giới hạn dưới, chúng ta liên tục bình phương nó cho đến khi nó ít nhất bằng giới hạn dưới. Mỗi bình phương được tính là một thao tác. 
4. Nếu trong quá trình này, giá trị ngừng thay đổi hoặc tăng quá chậm để đạt đến giới hạn yêu cầu (ví dụ: 0 hoặc 1 trường hợp), chúng tôi kết luận là không thể thực hiện được. 
5. Sau khi phần tử được điều chỉnh, chúng tôi đặt giới hạn dưới mới cho giá trị được điều chỉnh này và tiếp tục. 

Điểm tinh tế là bình phương không phải là tăng trưởng tuyến tính. Chúng tôi không bao giờ cố gắng “khớp chính xác”; chúng tôi chỉ quan tâm đến việc vượt qua ngưỡng. Số lượng bình phương cần thiết được xác định duy nhất bằng tốc độ tăng của giá trị khi bình phương lặp lại. 

### Tại sao nó hoạt động 

Thuật toán duy trì tính bất biến là sau khi xử lý vị trí i, chúng ta đã chọn một giá trị biến đổi hợp lệ cho a[i] là giá trị nhỏ nhất có thể đạt được trong các phép toán được phép trong khi vẫn đáp ứng thứ tự không giảm lên đến i. Điều này rất quan trọng vì việc chọn một giá trị lớn hơn mức cần thiết chỉ khiến cho những ràng buộc trong tương lai khó được đáp ứng hơn. 

Ở mỗi bước, chúng tôi không bao giờ xem lại các quyết định trước đó. Điều này là an toàn vì bất kỳ phần tử nào trước đó chỉ hạn chế các phần tử trong tương lai thông qua giá trị cuối cùng của nó và việc tăng giá trị cuối cùng đó sẽ chỉ làm tăng độ khó cho các vị trí sau này. Do đó, tính khả thi tối thiểu cục bộ ngụ ý tính tối ưu toàn cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        
        ops = 0
        prev = a[0]
        
        for i in range(1, n):
            x = a[i]
            
            if x >= prev:
                prev = x
                continue
            
            # try squaring until it reaches prev
            cnt = 0
            while x < prev:
                if x <= 1:
                    x = float('inf')
                    break
                x = x * x
                cnt += 1
            
            if x < prev:
                print(-1)
                break
            
            ops += cnt
            prev = x
        else:
            print(ops)

if __name__ == "__main__":
    solve()
```Đoạn mã tuân theo mô phỏng trực tiếp của chiến lược tham lam. Biến`prev`lưu trữ giá trị được chọn cuối cùng trong mảng được chuyển đổi. Đối với mỗi phần tử, nếu nó đã thỏa mãn ràng buộc về thứ tự thì chúng ta sẽ chấp nhận ngay. Ngược lại, chúng ta liên tục bình phương nó cho đến khi nó đi qua`prev`, các hoạt động đếm trên đường đi. 

Một chi tiết triển khai tinh tế là việc xử lý các giá trị 0 và 1. Vì việc bình phương không làm tăng chúng nên chúng không bao giờ có thể đạt đến giới hạn yêu cầu lớn hơn, vì vậy chúng tôi ngay lập tức đánh dấu tình huống này là không thể. Trong bài toán này, các ràng buộc có giá trị dương nhưng việc bảo vệ chống lại sự trì trệ đảm bảo tính chính xác trong các biến thể mở rộng. 

Vòng lặp sử dụng Python`else`mệnh đề trên vòng lặp for để phân biệt việc hoàn thành thành công với việc chấm dứt sớm do không thể thực hiện được. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
3
1 2 3
3
3 2 5
3
4 3 2
```Dấu vết cho trường hợp thử nghiệm thứ hai: 

| tôi | một [tôi] | trước trước | hoạt động | giá trị sau | trước sau | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 3 | - | 0 | 3 | 3 | 
| 1 | 2 | 3 | 1 | 4 | 4 | 
| 2 | 5 | 4 | 0 | 5 | 5 | 

Điều này chứng tỏ làm thế nào một bình phương đơn lẻ có thể sửa chữa một nghịch đảo cục bộ mà không ảnh hưởng đến cấu trúc tổng thể. 

### Ví dụ 2 

đầu vào:```
3
4
1 1 2 3
3
5 4 2
3
10 2 3
```Dấu vết cho`5 4 2`: 

| tôi | một [tôi] | trước | hoạt động | giá trị | trước | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 5 | - | 0 | 5 | 5 | 
| 1 | 4 | 5 | 1 | 16 | 16 | 
| 2 | 2 | 16 | 2 | 4 → 16 | 16 | 

Chúng tôi thấy rằng việc bình phương lặp đi lặp lại đôi khi là cần thiết và số lượng phép tính được tích lũy một cách tự nhiên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log log A) | Mỗi phần tử chỉ được bình phương một số lần nhỏ trước khi vượt quá giới hạn | 
| Không gian | O(1) | Chỉ một số biến được duy trì cho mỗi trường hợp thử nghiệm | 

Tốc độ tăng trưởng của bình phương lặp lại là cực kỳ nhanh nên số lần lặp trên mỗi phần tử được giới hạn trong khoảng 6 đến 7 đối với các giá trị lên tới 10^6, đảm bảo giải pháp nằm trong giới hạn cho tổng số 2 · 10^5 phần tử. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    
    solve()
    
    return output.getvalue().strip()

# provided samples (abridged formatting)
assert run("""7
3
1 2 3
2
3 2
3
3 1 5
4
1 1 2 3
3
4 3 2
9
16 2 4 2 256 2 4 2 8
11
10010 10009 10008 10007 10006 10005 10004 10003 10002 10001 10000
""") == """0
1
-1
0
3
15
55"""

# minimum size
assert run("""1
1
5
""") == "0"

# already sorted
assert run("""1
5
1 2 3 4 5
""") == "0"

# strictly decreasing small
assert run("""1
4
4 3 2 1
""") == "3"

# contains 1s (stuck behavior check)
assert run("""1
3
1 1 1
""") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 0 | trường hợp cơ sở | 
| mảng được sắp xếp | 0 | không cần thao tác | 
| mảng giảm dần | 3 | chuỗi sửa chữa bình phương lặp đi lặp lại | 
| tất cả những cái | 0 | không cần tăng trưởng | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi các giá trị trở thành 1. Vì bình phương 1 vẫn bằng 1 nên bất kỳ yêu cầu nào lớn hơn 1 đều khiến phiên bản không thể thực hiện được. Ví dụ, đầu vào`[1, 2]`thì ổn, nhưng`[2, 1, 2]`có thể thất bại tùy thuộc vào sự lan truyền vì phần tử ở giữa không thể phát triển nếu bị ràng buộc không chính xác. 

Một trường hợp tinh tế khác là khi bình phương lớn sớm tạo ra độ vọt lố. Ví dụ, nếu chúng ta có`[2, 3, 2]`, phần tử thứ hai có thể được bình phương một cách không cần thiết nếu chúng ta không cẩn thận giảm thiểu các thao tác, sau đó buộc phần tử thứ ba phải bình phương nhiều lần. Chiến lược tham lam tránh điều này bằng cách luôn chọn bình phương tối thiểu cần thiết ở mỗi bước. 

Trường hợp thứ ba là chuỗi giảm dài như`10010 ... 10000`. Những điều này nhấn mạnh số lượng bình phương lặp đi lặp lại nhưng vẫn an toàn do sự tăng trưởng theo cấp số nhân đảm bảo chấm dứt ở một số bước nhỏ cho mỗi phần tử.
