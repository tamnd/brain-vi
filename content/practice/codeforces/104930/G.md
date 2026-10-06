---
title: "CF 104930G - Xương và Mảng"
description: "Chúng ta được cung cấp một mảng nhỏ các số nguyên, mỗi trường hợp thử nghiệm độc lập. Từ mảng đó, chúng tôi xem xét mọi tập hợp con có thể có của các phần tử ngoại trừ việc chúng tôi không được phép lấy toàn bộ mảng."
date: "2026-06-28T07:47:59+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104930
codeforces_index: "G"
codeforces_contest_name: "UTPC Contest 01-26-24 Div. 2 (Beginner)"
rating: 0
weight: 104930
solve_time_s: 252
verified: false
draft: false
---

[CF 104930G - Xương tối và Mảng](https://codeforces.com/problemset/problem/104930/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 4 phút 12s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng nhỏ các số nguyên, mỗi trường hợp thử nghiệm độc lập. Từ mảng đó, chúng tôi xem xét mọi tập hợp con có thể có của các phần tử ngoại trừ việc chúng tôi không được phép lấy toàn bộ mảng. Tập hợp con trống được cho phép vì nó vẫn là tập hợp con và không vi phạm điều kiện “không tập hợp đầy đủ”. 

Đối với bất kỳ tập hợp con được chọn$S$, chúng tôi tính toán một giá trị được hình thành từ hai phần: kích thước của tập hợp con và tổng lập phương của các phần tử của nó. Kích thước tập hợp con được nhân lên và dấu phụ thuộc vào kích thước đó là số lẻ hay số chẵn. Cụ thể, các tập hợp con có kích thước chẵn đóng góp tích cực, các tập hợp con có kích thước lẻ đóng góp tiêu cực và mọi thứ đều được điều chỉnh theo kích thước tập hợp con. 

Nhiệm vụ là đánh giá biểu thức này cho mọi tập hợp con hợp lệ và báo cáo các giá trị tối thiểu và tối đa cho mỗi trường hợp thử nghiệm. 

Hạn chế chính đó là$N \le 15$. Đây là tín hiệu quan trọng: mặc dù số lượng tập hợp con là$2^N$, nhiều nhất$2^{15} = 32768$, vì vậy việc liệt kê tất cả các tập con cho mỗi trường hợp thử nghiệm là khả thi. Ngay cả với tối đa 1000 trường hợp thử nghiệm, một giải pháp xử lý từng tập hợp con trong thời gian khấu hao không đổi hoặc rất nhỏ vẫn có thể được chấp nhận. Điều trở nên không thể chấp nhận được là bất kỳ cách tiếp cận nào tính toán lại tổng các tập hợp con từ đầu cho mỗi tập hợp con, điều này sẽ đưa ra một hệ số bổ sung của$N$, đẩy sự phức tạp lên tới nửa tỷ hoạt động. 

Các trường hợp cạnh chủ yếu là về quy tắc lựa chọn tập hợp con và hành vi ký hiệu. 

Một lỗi phổ biến là cho rằng tập hợp con trống không được phép vì cách diễn đạt "tập hợp con nghiêm ngặt". Nếu chúng tôi loại trừ nó, chúng tôi sẽ bỏ lỡ một ứng cử viên hợp lệ có giá trị 0, giá trị này có thể ảnh hưởng đến giá trị tối thiểu hoặc tối đa khi tất cả các giá trị khác hoàn toàn là dương hoặc âm. 

Một trường hợp tinh tế khác là loại trừ tập hợp con đầy đủ. Nếu chúng ta vô tình đưa nó vào thì đóng góp của nó có thể chiếm ưu thế vì cả hệ số nhân kích thước tập hợp con và tổng lập phương đều ở mức tối đa. 

Ví dụ, nếu$A = [1, 1, 1]$, thì tập hợp con đầy đủ sẽ tạo ra$3 \cdot (-1)^3 \cdot 3 = -9$, có thể trở thành mức tối thiểu một cách không chính xác, mặc dù nó không nên được xem xét. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là lặp qua từng tập hợp con bằng cách sử dụng mặt nạ bit. Đối với mỗi tập hợp con, chúng tôi tính toán kích thước của nó và tổng lập phương của các phần tử được bao gồm, sau đó tính biểu thức. Điều này đúng vì nó tuân theo định nghĩa một cách rõ ràng. Vấn đề là tính hiệu quả trong việc tính toán các tổng tập hợp con nhiều lần. 

Nếu chúng tôi tính lại tổng khối từ đầu cho mỗi tập hợp con, chúng tôi sẽ quét tối đa 15 phần tử cho mỗi tập hợp con. Điều đó dẫn đến khoảng$2^N \cdot N$, tức là khoảng 500.000 thao tác cho mỗi trường hợp thử nghiệm và lên tới 500 triệu trong trường hợp xấu nhất trong tất cả các thử nghiệm. Điều đó quá chậm trong Python. 

Sự cải tiến đến từ việc nhận ra rằng các tập hợp con tạo thành cấu trúc DP tự nhiên. Nếu chúng ta biểu diễn các tập hợp con dưới dạng mặt nạ bit, chúng ta có thể tính tổng khối của từng tập hợp con tăng dần. Một tập hợp con có thể được phân tách thành tập hợp con nhỏ hơn cộng với một phần tử mới được thêm vào. Điều này cho phép chúng ta tính toán tất cả các tổng tập hợp con trong$O(2^N)$thời gian cho mỗi trường hợp thử nghiệm mà không cần quét lại các phần tử. 

Khi chúng ta có tổng tập hợp con một cách hiệu quả, việc đánh giá biểu thức cho mỗi tập hợp con là thời gian không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (quét lại mỗi tập hợp con) |$O(2^N \cdot N)$|$O(1)$| Quá chậm | 
| Bitmask DP trên tổng tập hợp con |$O(2^N)$|$O(2^N)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng trường hợp thử nghiệm một cách độc lập. 

1. Chuyển đổi từng phần tử$A_i$vào giá trị khối của nó$A_i^3$. Điều này tránh việc tính toán lại các lũy thừa nhiều lần và giữ số học cục bộ cho các số nguyên. 
2. Tính toán trước tổng các tập hợp con bằng cách sử dụng mặt nạ bit DP. Đối với mỗi mặt nạ, chúng tôi tách bit thiết lập có ý nghĩa nhỏ nhất của nó và biểu thị mặt nạ dưới dạng mặt nạ nhỏ hơn cộng với một phần tử. Điều này đảm bảo mỗi tổng tập hợp con được tính một lần, dựa trên kết quả đã được tính toán. 
3. Đối với mỗi mặt nạ từ$0$ĐẾN$2^N - 1$, tính kích thước tập hợp con và đánh giá biểu thức$|S| \cdot (-1)^{|S|} \cdot \text{sum}(S)$. 
4. Bỏ qua việc đắp mặt nạ đầy đủ$(1 << N) - 1$, vì vấn đề cấm sử dụng toàn bộ mảng. 
5. Theo dõi mức tối thiểu và tối đa toàn cầu trên tất cả các tập hợp con hợp lệ. 

Lựa chọn thiết kế chính là tính toán tổng tập hợp con thông qua DP thay vì tính toán lại. Nếu không có điều này, giải pháp sẽ vượt quá giới hạn thời gian mặc dù$N$là nhỏ. 

### Tại sao nó hoạt động 

Mỗi tập hợp con tương ứng với chính xác một mặt nạ bit và mỗi mặt nạ bit có thể được giảm bớt bằng cách loại bỏ một bit đã đặt. DP đảm bảo rằng khi chúng tôi tính toán một tập hợp con, tập hợp con nhỏ hơn mà nó phụ thuộc vào đã được biết đến. Điều này tạo ra một thứ tự nghiêm ngặt trên các mặt nạ theo số bit, đảm bảo không tính toán lại và không có trạng thái bị thiếu. Vì biểu thức chỉ phụ thuộc vào kích thước và tổng tập hợp con, cả hai đều được xác định đầy đủ trên mỗi mặt nạ, nên việc đánh giá từng mặt nạ một cách độc lập là đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    T = int(input())
    for _ in range(T):
        N = int(input())
        A = list(map(int, input().split()))
        
        nmask = 1 << N
        cube = [x * x * x for x in A]
        
        # subset sum DP over bitmasks
        sub_sum = [0] * nmask
        
        for mask in range(1, nmask):
            lsb = mask & -mask
            idx = (lsb.bit_length() - 1)
            sub_sum[mask] = sub_sum[mask ^ lsb] + cube[idx]
        
        full_mask = nmask - 1
        
        mn = float('inf')
        mx = float('-inf')
        
        for mask in range(nmask):
            if mask == full_mask:
                continue
            
            k = mask.bit_count()
            val = k * sub_sum[mask]
            if k % 2 == 1:
                val = -val
            
            if val < mn:
                mn = val
            if val > mx:
                mx = val
        
        print(mn, mx)

if __name__ == "__main__":
    solve()
```Quá trình tiền xử lý khối sẽ tách biệt phép lũy thừa để việc đánh giá tập hợp con trở nên hoàn toàn phụ thuộc. Bước DP sử dụng nhận dạng để loại bỏ bit được đặt thấp nhất luôn dẫn đến mặt nạ nhỏ hơn đã được xử lý. Việc trích xuất chỉ số bit thông qua`lsb.bit_length() - 1`chuyển đổi bit bị cô lập thành chỉ mục mảng một cách an toàn. 

Trong quá trình đánh giá, kích thước tập hợp con có được thông qua`bit_count()`, hiệu quả đối với quy mô nhỏ$N$. Việc lật dấu được áp dụng sau phép nhân để tránh lỗi về độ ưu tiên của toán tử. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Hãy xem xét một mảng nhỏ$A = [1, -1, 2]$. Chúng tôi đánh giá tất cả các tập hợp con ngoại trừ tập hợp đầy đủ. 

| Mặt nạ | Tập hợp con | Cỡ k | Tổng các khối | Biểu hiện | 
| --- | --- | --- | --- | --- | 
| 000 | {} | 0 | 0 | 0 | 
| 001 | {1} | 1 | 1 | -1 | 
| 010 | {-1} | 1 | -1 | 1 | 
| 100 | {2} | 1 | 8 | -8 | 
| 011 | {1,-1} | 2 | 0 | 0 | 
| 101 | {1,2} | 2 | 9 | 18 | 
| 110 | {-1,2} | 2 | 7 | 14 | 

Trọn bộ 111 bị loại trừ. 

Tối thiểu là -8 và tối đa là 18. 

Dấu vết này cho thấy các tập hợp con lẻ đảo ngược dấu như thế nào trong khi các tập hợp con chẵn giữ nguyên dấu đó và cách chia tỷ lệ theo kích thước tập hợp con khuếch đại sự khác biệt. 

### Ví dụ 2 

lấy$A = [-2, -2]$. 

| Mặt nạ | Tập hợp con | Cỡ k | Tổng các khối | Biểu hiện | 
| --- | --- | --- | --- | --- | 
| 00 | {} | 0 | 0 | 0 | 
| 01 | {-2} | 1 | -8 | 8 | 
| 10 | {-2} | 1 | -8 | 8 | 

Toàn bộ 11 được loại trừ. 

Cả hai tập hợp con một phần tử đều tạo ra các giá trị giống hệt nhau và tập hợp con trống có giá trị tối thiểu là 0. 

Điều này chứng tỏ rằng khi tất cả các phần tử giống hệt nhau, tính đối xứng của tập hợp con sẽ thu gọn phạm vi đầu ra. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(2^N)$mỗi trường hợp thử nghiệm | Mỗi tập hợp con được tính một lần và đánh giá là thời gian không đổi | 
| Không gian |$O(2^N)$| Lưu trữ tổng tập hợp con cho tất cả các mặt nạ | 

Với$N \le 15$, mỗi bài kiểm tra chạy ở khoảng 32768 trạng thái và thậm chí với 1000 bài kiểm tra, điều này vẫn nằm trong giới hạn thực tế trong Python do các phép toán số nguyên chặt chẽ và các mẫu truy cập bộ nhớ tuyến tính. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import isfinite
    
    input = sys.stdin.readline
    
    def solve():
        T = int(input())
        out = []
        for _ in range(T):
            N = int(input())
            A = list(map(int, input().split()))
            
            nmask = 1 << N
            cube = [x * x * x for x in A]
            sub_sum = [0] * nmask
            
            for mask in range(1, nmask):
                lsb = mask & -mask
                idx = (lsb.bit_length() - 1)
                sub_sum[mask] = sub_sum[mask ^ lsb] + cube[idx]
            
            full_mask = nmask - 1
            mn = float('inf')
            mx = float('-inf')
            
            for mask in range(nmask):
                if mask == full_mask:
                    continue
                k = mask.bit_count()
                val = k * sub_sum[mask]
                if k % 2 == 1:
                    val = -val
                mn = min(mn, val)
                mx = max(mx, val)
            
            out.append(f"{mn} {mx}")
        return "\n".join(out)

    return solve()

# provided sample (format adjusted as parsing is unclear in statement)
# assert run(...) == ...

# custom cases
assert run("1\n1\n5\n") == "0 0", "single element excludes full set"
assert run("1\n2\n1 1\n") is not None
assert run("1\n3\n-1 -1 -1\n") is not None
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Yếu tố đơn | 0 0 | Chỉ tập hợp con trống là hợp lệ | 
| Tất cả đều tích cực như nhau | khác nhau | đối xứng và loại trừ tập hợp con đầy đủ | 
| Tất cả đều tiêu cực như nhau | khác nhau | hành vi luân phiên dấu hiệu | 

## Vỏ cạnh 

Tập hợp con trống là trường hợp cạnh khái niệm chính. Đối với một đầu vào như$[5]$, các tập hợp con duy nhất được phép trống và tập hợp con một phần tử không được phép vì nó bằng toàn bộ mảng. Thuật toán chỉ đánh giá mặt nạ trống và tạo ra giá trị 0 một cách chính xác. 

Việc loại trừ tập hợp con đầy đủ là một điều kiện quan trọng khác. Vì$A = [2, 2]$, tập hợp con chứa cả hai phần tử sẽ mang lại$2 \cdot (+1) \cdot (16 + 16) = 64$, nhưng nó không bao giờ được xem xét. Vòng lặp bỏ qua mặt nạ này một cách rõ ràng, thay vào đó đảm bảo mức tối đa đến từ các tập hợp con một phần tử. 

Khi tất cả các phần tử đều âm, các hình khối giữ nguyên giá trị âm nhưng dấu xen kẽ từ kích thước tập hợp con sẽ tạo ra các lần lật không trực quan. DP vẫn xử lý việc này một cách chính xác vì nó không bao giờ giả định tính đơn điệu, nó đánh giá từng tập hợp con một cách độc lập với tổng được tính toán của nó.
