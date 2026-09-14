---
title: "CF 104673D - Tạp chí"
description: "Chúng ta có một chồng tạp chí được biểu thị bằng một chuỗi + và -, trong đó mỗi ký hiệu mô tả hướng của bìa tạp chí. Ngăn xếp được đọc từ trên xuống dưới khi chuỗi được đưa ra."
date: "2026-06-29T09:19:23+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104673
codeforces_index: "D"
codeforces_contest_name: "2022-2023 CTU Open Contest"
rating: 0
weight: 104673
solve_time_s: 52
verified: true
draft: false
---

[CF 104673D - Tạp chí](https://codeforces.com/problemset/problem/104673/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 52s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một chồng các tạp chí được biểu diễn bằng một chuỗi các`+`Và`-`, trong đó mỗi ký hiệu mô tả hướng của bìa tạp chí. Ngăn xếp được đọc từ trên xuống dưới khi chuỗi được đưa ra. Mục tiêu là sắp xếp lại ngăn xếp sao cho không có hai tạp chí liền kề nào có cùng dấu, nghĩa là chuỗi cuối cùng phải xen kẽ chặt chẽ giữa`+`Và`-`. 

Hoạt động được phép có cấu trúc nhưng hiệu quả mang lại sự chuyển đổi mạnh mẽ. Chúng ta có thể loại bỏ một số tiền tố khỏi đầu ngăn xếp, sau đó lấy một khối liền kề từ những gì còn lại, đảo ngược khối đó và cuối cùng đặt tiền tố đã bị loại bỏ trở lại trên cùng. Quan sát chính là điều này cho phép đảo ngược bất kỳ chuỗi con liền kề nào của ngăn xếp trong một thao tác. 

Vì việc đảo ngược là thay đổi thực sự duy nhất nên vấn đề trở thành một nhiệm vụ chuyển đổi: chuyển đổi chuỗi nhị phân ban đầu thành chuỗi mục tiêu xen kẽ bằng cách sử dụng số lượng đảo ngược chuỗi con tối thiểu. 

Độ dài đầu vào lên tới$2 \cdot 10^5$, điều này ngay lập tức loại trừ bất kỳ giải pháp nào thử tất cả các chuỗi con hoặc mô phỏng từng thao tác một cách tham lam trong thời gian bậc hai. Chúng ta cần một chiến lược tuyến tính hoặc gần tuyến tính. 

Một điểm tinh tế là có chính xác hai cấu hình mục tiêu hợp lệ: một bắt đầu bằng`+`và một bắt đầu bằng`-`. Cả hai đều là các mẫu xen kẽ hợp lệ và chúng ta phải chọn mẫu yêu cầu ít thao tác hơn. 

Các trường hợp cạnh quan trọng là các chuỗi nhỏ và các chuỗi đã chính xác. Ví dụ, đầu vào`"+-"`đã thỏa mãn điều kiện nên câu trả lời là 0. Đối với một chuỗi như`"++--"`, một cách tiếp cận ngây thơ có thể cố gắng giải quyết các xung đột cục bộ một cách tham lam và các hoạt động đếm quá mức, mặc dù một sự đảo ngược duy nhất của đoạn giữa sẽ tạo ra`"+-+-"`. 

## Phương pháp tiếp cận 

Chế độ xem brute-force là coi mỗi trạng thái là một chuỗi và thử tất cả các lần đảo ngược chuỗi con có thể có, thực hiện BFS trên các cấu hình. Mỗi tiểu bang có$O(n^2)$hàng xóm, và ngay cả việc thăm dò độ sâu nhỏ cũng trở nên không khả thi vì số lượng chuỗi có thể truy cập tăng lên bùng nổ. Ngay cả khi chúng ta giới hạn bản thân ở những đường đi tối ưu, không gian trạng thái vẫn$2^{2K}$, điều này hoàn toàn không thể quản lý được. 

Thông tin chi tiết quan trọng là chúng tôi không cố gắng đạt được một cấu hình tùy ý mà là một trong hai mẫu xen kẽ cố định. Điều này loại bỏ sự cần thiết phải suy luận về cấu trúc toàn cầu. Thay vào đó, chúng tôi so sánh chuỗi hiện tại với chuỗi đích và tập trung vào các vị trí không khớp. 

Một sự đảo ngược có thể sửa chữa hai ranh giới không khớp trong một nước đi nếu chúng ta chọn điểm cuối của nó một cách cẩn thận. Theo trực giác, bất cứ khi nào hai vị trí sai so với mục tiêu, chúng ta có thể ghép chúng và sửa cả hai trong một lần đảo ngược. Điều này làm giảm vấn đề đếm những điểm không khớp và ghép chúng một cách tối ưu. Mỗi thao tác có thể loại bỏ tối đa hai điểm không khớp và một sự đảo ngược được lựa chọn cẩn thận luôn có thể đạt được sự kết hợp này mà không phá vỡ cấu trúc cố định trước đó. 

Điều này dẫn đến một cách giảm đơn giản: tính toán các điểm không khớp đối với từng mục tiêu trong số hai mục tiêu xen kẽ và thu được kết quả tốt hơn. Câu trả lời trở thành số lượng không khớp chia cho hai, làm tròn lên. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| BFS qua các tiểu bang | hàm mũ | hàm mũ | Quá chậm | 
| Ghép nối không khớp | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi giải quyết vấn đề bằng cách đánh giá cả hai mục tiêu xen kẽ có thể có và đếm xem có bao nhiêu quan điểm không đồng ý với từng mục tiêu. 

1. Xây dựng chuỗi mục tiêu giả định bắt đầu bằng`+`, xen kẽ trong toàn bộ chiều dài. 

Điều này thể hiện một cấu hình cuối cùng hợp lệ. 
2. So sánh chuỗi đầu vào với mục tiêu này và đếm xem có bao nhiêu chỉ số khác nhau. 

Mỗi vị trí khác nhau đại diện cho một tạp chí hiện đang có định hướng sai so với mục tiêu. 
3. Tính toán số thao tác cần thiết cho mục tiêu này như sau:`(mismatch_count + 1) // 2`. 

Điều này xuất phát từ thực tế là một lần đảo chiều có thể khắc phục được hai vị trí không khớp khi được ghép nối chính xác. 
4. Lặp lại quy trình tương tự cho mục tiêu bắt đầu bằng`-`. 
5. Trả về giá trị nhỏ nhất giữa hai kết quả tính toán. 

### Tại sao nó hoạt động 

Đặc tính quan trọng là các vị trí không khớp là độc lập ngoại trừ việc ghép cặp thông qua các điểm đảo chiều. Một lần đảo chiều duy nhất chỉ có thể "kết hợp" các điều chỉnh một cách có ý nghĩa ở các điểm cuối của nó trong khi vẫn giữ cấu trúc bên trong nhất quán với mẫu mục tiêu. Vì mọi thao tác có thể khắc phục tối đa hai điểm không khớp và bất kỳ thao tác không khớp nào cũng yêu cầu ít nhất một thao tác, nên chiến lược tối ưu tương đương với việc ghép các điểm không khớp một cách hiệu quả nhất có thể. Việc thử cả hai mẫu mục tiêu đảm bảo chúng ta không thiên về điểm chẵn lẻ ban đầu sai. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    s = input().strip()
    n = len(s)

    def cost(start_char):
        mismatches = 0
        cur = start_char
        for i in range(n):
            if s[i] != cur:
                mismatches += 1
            cur = '+' if cur == '-' else '-'
        return (mismatches + 1) // 2

    ans1 = cost('+')
    ans2 = cost('-')
    print(min(ans1, ans2))

if __name__ == "__main__":
    solve()
```Việc triển khai trực tiếp mã hóa hai mẫu xen kẽ ứng cử viên. Hàm trợ giúp duyệt qua chuỗi một lần, duy trì ký tự mong muốn ở mỗi vị trí. Số lượng không khớp được tích lũy trong một lần chuyển, giữ cho giải pháp tuyến tính. 

biểu thức`(mismatches + 1) // 2`nắm bắt đối số ghép nối: mọi thao tác có thể giải quyết hai vị trí không chính xác và nếu một vị trí vẫn chưa được ghép nối, nó sẽ yêu cầu một thao tác bổ sung. 

## Ví dụ đã hoạt động 

Hãy xem xét một đầu vào đơn giản như`"+-+-"`. Số lượng không khớp với cùng một mẫu là 0, do đó không cần thực hiện thao tác nào. 

Để có một trường hợp minh họa hơn, hãy lấy`"++--"`. 

### Mục tiêu bắt đầu bằng`+`| tôi | s[i] | mục tiêu | không khớp | 
| --- | --- | --- | --- | 
| 0 | + | + | 0 | 
| 1 | + | - | 1 | 
| 2 | - | + | 1 | 
| 3 | - | - | 0 | 

Số lượng không khớp là 2, vì vậy các thao tác cần thiết là 1. 

### Mục tiêu bắt đầu bằng`-`| tôi | s[i] | mục tiêu | không khớp | 
| --- | --- | --- | --- | 
| 0 | + | - | 1 | 
| 1 | + | + | 0 | 
| 2 | - | - | 0 | 
| 3 | - | + | 1 | 

Số lượng không khớp lại là 2, cho 1 thao tác. 

Điều này cho thấy tính đối xứng: cả hai hướng đích đều có giá trị như nhau và thuật toán chọn chính xác một trong hai hướng đó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi trong số hai kiểm tra mục tiêu sẽ quét chuỗi một lần | 
| Không gian | O(1) | Chỉ sử dụng bộ đếm và một vài biến | 

Giải pháp xử lý thoải mái các chuỗi có độ dài lên đến$2 \cdot 10^5$bởi vì nó chỉ thực hiện một số lượng tuyến tính không đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    s = input().strip()
    n = len(s)

    def cost(start):
        mismatches = 0
        cur = start
        for i in range(n):
            if s[i] != cur:
                mismatches += 1
            cur = '+' if cur == '-' else '-'
        return (mismatches + 1) // 2

    return str(min(cost('+'), cost('-')))

# provided-style samples
assert run("+-+-\n") == "0"
assert run("++--\n") == "1"

# custom cases
assert run("+\n") == "0"
assert run("++\n") == "1"
assert run("-+-+\n") == "0"
assert run("++++----\n") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`"+"`|`0`| Phần tử đơn đã hợp lệ | 
|`"++"`|`1`| Chỉnh sửa tối thiểu không tầm thường | 
|`"-+-+"`|`0`| Đã xen kẽ bắt đầu bằng`-`| 
|`"++++----"`|`2`| Cấu trúc khối lớn hơn | 

## Vỏ cạnh 

Đối với chuỗi ký tự đơn như`"+"`, cả hai mẫu mục tiêu đều hợp lệ cho đến việc cắt ngắn quy tắc xen kẽ và số lượng không khớp bằng 0, do đó thuật toán trả về 0 ngay lập tức. 

Đối với một chuỗi đã xen kẽ như`"+-+-+-"`, số lượng không khớp bằng 0 so với một trong các mục tiêu, do đó không cần thực hiện thao tác nào và thuật toán sẽ chọn mục tiêu đó một cách tự nhiên. 

Đối với một chuỗi thống nhất như`"++++"`, cả hai mục tiêu đều tạo ra hai điểm không khớp cho mỗi hai ký tự, dẫn đến tổng cộng hai điểm không khớp và do đó có một thao tác, tương ứng với việc đảo ngược đoạn giữa để tạo ra sự thay thế.
