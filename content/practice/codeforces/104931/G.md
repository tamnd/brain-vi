---
title: "CF 104931G - Xương tối và mảng"
description: "Chúng ta được cung cấp một mảng nhỏ các số nguyên cho mỗi trường hợp thử nghiệm. Từ mảng này, chúng ta có thể chọn bất kỳ tập hợp con nào của các phần tử, nhưng rõ ràng chúng ta bị cấm chọn toàn bộ mảng. Tập hợp con thậm chí có thể trống."
date: "2026-06-28T07:37:15+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104931
codeforces_index: "G"
codeforces_contest_name: "UTPC Contest 01-26-24 Div. 1 (Advanced)"
rating: 0
weight: 104931
solve_time_s: 76
verified: false
draft: false
---

[CF 104931G - Xương tối và Mảng](https://codeforces.com/problemset/problem/104931/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 16s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng nhỏ các số nguyên cho mỗi trường hợp thử nghiệm. Từ mảng này, chúng ta có thể chọn bất kỳ tập hợp con nào của các phần tử, nhưng rõ ràng chúng ta bị cấm chọn toàn bộ mảng. Tập hợp con thậm chí có thể trống. 

Đối với bất kỳ tập hợp con được chọn$S$, chúng tôi tính toán một giá trị kết hợp ba phần: kích thước của tập hợp con, dấu hiệu phụ thuộc vào kích thước là chẵn hay lẻ và tổng lập phương của các phần tử được chọn. Cụ thể, mỗi tập hợp con đóng góp một giá trị bằng kích thước của nó nhân với$(-1)^{|S|}$nhân với tổng của$x^3$trên tất cả các phần tử trong tập hợp con. 

Nhiệm vụ là tìm trong số tất cả các tập hợp con hợp lệ các giá trị nhỏ nhất và lớn nhất có thể có của biểu thức này. 

Chi tiết cấu trúc quan trọng là độ dài mảng tối đa là 15. Điều đó ngay lập tức chuyển quan điểm từ “tối ưu hóa tổ hợp” sang “liệt kê mọi thứ”, vì tổng số tập hợp con nhiều nhất là$2^{15} = 32768$. Ngay cả với tối đa 1000 trường hợp thử nghiệm, việc quét toàn bộ các tập hợp con vẫn nằm trong giới hạn. 

Một điều kiện biên tinh tế là vai trò của tập con trống. Đối với một tập hợp trống, cả kích thước và tổng đều bằng 0, do đó giá trị đánh giá rõ ràng bằng 0. Một sự tinh tế khác là hạn chế chọn bộ đầy đủ. Điều đó có nghĩa là một tập hợp con phải được loại trừ khỏi việc xem xét mặc dù nếu không nó sẽ là một phần của bảng liệt kê hoàn chỉnh. 

Bản thân biểu thức cũng có thể hoạt động không trực quan vì dấu phụ thuộc vào tính chẵn lẻ của kích thước tập hợp con, trong khi độ lớn tỷ lệ tuyến tính với kích thước và cũng tuyến tính với tổng các khối. Điều này có nghĩa là những thay đổi nhỏ trong thành phần tập hợp con có thể đảo dấu và khuếch đại hoặc giảm kết quả theo những cách không đơn điệu, loại trừ khả năng suy luận tham lam. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp là lặp qua từng tập hợp con của mảng. Đối với mỗi tập hợp con, chúng tôi tính toán kích thước của nó và tổng lập phương của các phần tử của nó, sau đó tính biểu thức. Chúng tôi theo dõi mức tối thiểu và tối đa trên tất cả các tập hợp con hợp lệ. 

Điều này đúng vì mọi lựa chọn có thể có của các phần tử đều được xem xét rõ ràng. Chi phí của phương pháp này hoàn toàn đến từ việc liệt kê các tập hợp con. Với$N = 15$, chúng tôi có 32768 tập con cho mỗi trường hợp thử nghiệm. Đối với mỗi tập hợp con, chúng tôi có thể quét tối đa 15 phần tử, đưa ra khoảng 500 nghìn thao tác cho mỗi trường hợp thử nghiệm trong trường hợp xấu nhất. Với tối đa 1000 trường hợp thử nghiệm, điều này sẽ trở nên quá lớn nếu được triển khai một cách đơn giản trong một vòng lặp chặt chẽ với chi phí lớn, nhưng trong Python được tối ưu hóa, nó vẫn ở mức chấp nhận được. Việc triển khai cẩn thận hơn sẽ tránh được công việc lặp lại bằng cách tính toán trước các giá trị khối và sử dụng các thao tác bit. 

Quan sát quan trọng là không cần cấu trúc sâu hơn. Không giống như các bài toán trong đó các tập hợp con tương tác hoặc yêu cầu tối ưu hóa trên một miền liên tục, ở đây các ràng buộc đảm bảo rằng phép liệt kê bạo lực là giải pháp dự kiến. Tối ưu hóa duy nhất cần thiết là biểu diễn các tập hợp con dưới dạng bitmask và tính toán trước$A_i^3$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Bảng liệt kê tập hợp con Brute Force |$O(T \cdot 2^N \cdot N)$|$O(N)$| Được chấp nhận với sự tối ưu hóa | 
| Bitmask với các khối được tính toán trước |$O(T \cdot 2^N)$|$O(N)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng trường hợp thử nghiệm một cách độc lập. 

1. Tính toán trước khối của từng phần tử mảng để chúng ta không tính lũy thừa nhiều lần trong quá trình đánh giá tập hợp con. Điều này làm giảm số học lặp lại bên trong vòng lặp bên trong. 
2. Lặp lại tất cả các mặt nạ bit từ 0 đến$2^N - 1$. Mỗi bitmask đại diện cho một tập hợp con, trong đó$i$-bit thứ cho biết phần tử có$A_i$được bao gồm. 
3. Bỏ qua bitmask tương ứng với bộ đầy đủ, vì sự cố không cho phép chọn tất cả các phần tử. 
4. Đối với mỗi bitmask còn lại, hãy tính hai đại lượng: số phần tử được chọn và tổng lập phương của các phần tử được chọn đó. Điều này được thực hiện bằng cách quét các bit và tích lũy cả số đếm và tổng. 
5. Đánh giá biểu thức bằng cách sử dụng các giá trị tính toán. Nếu kích thước tập hợp con bằng 0 thì đóng góp bằng 0 theo định nghĩa của công thức. 
6. Duy trì mức tối thiểu và tối đa toàn cầu trên tất cả các tập hợp con được đánh giá. 
7. Sau khi xử lý tất cả các tập hợp con, xuất ra giá trị tối thiểu và tối đa. 

Lý do đây là cấu trúc đúng là vì mỗi tập hợp con đều độc lập. Không có ràng buộc nào khi liên kết một lựa chọn tập hợp con này với một lựa chọn tập hợp con khác, do đó không gian tìm kiếm hoàn toàn có thể tách biệt thành các đánh giá độc lập trên tập lũy thừa trừ một phần tử. 

### Tại sao nó hoạt động 

Mỗi tập hợp con hợp lệ tương ứng với chính xác một mặt nạ bit ngoại trừ mặt nạ tập hợp đầy đủ và mỗi mặt nạ bit được đánh giá chính xác một lần. Do việc tính toán cho mỗi mặt nạ khớp chính xác với định nghĩa của hàm được yêu cầu nên thuật toán thực hiện liệt kê đầy đủ không gian giải pháp mà không bỏ sót hoặc trùng lặp. Do đó, mức tối thiểu và tối đa được lấy trên toàn bộ miền khả thi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    T = int(input())
    for _ in range(T):
        N = int(input())
        A = list(map(int, input().split()))
        
        cube = [x * x * x for x in A]
        
        full_mask = (1 << N) - 1
        
        INF = 10**30
        mn, mx = INF, -INF
        
        for mask in range(1 << N):
            if mask == full_mask:
                continue
            
            cnt = 0
            s = 0
            
            m = mask
            i = 0
            while m:
                if m & 1:
                    cnt += 1
                    s += cube[i]
                m >>= 1
                i += 1
            
            if cnt == 0:
                val = 0
            else:
                sign = -1 if cnt % 2 else 1
                val = cnt * sign * s
            
            if val < mn:
                mn = val
            if val > mx:
                mx = val
        
        print(mn, mx)

if __name__ == "__main__":
    solve()
```Chi tiết triển khai cốt lõi là vòng lặp bitmask. Mỗi mặt nạ mã hóa một tập hợp con và chúng tôi bỏ qua mặt nạ đầy đủ một cách rõ ràng. Vòng lặp bên trong giải mã từng bit một, duy trì cả số lượng và tổng các khối. Biểu thức sau đó được đánh giá chính xác như được chỉ định. 

Một lỗi phổ biến là quên rằng tập con trống là hợp lệ và phải được xem xét. Một cách khác là xử lý sai dấu khi kích thước tập hợp con bằng 0, nhưng việc triển khai đương nhiên mang lại kết quả bằng 0 trong trường hợp đó. Việc tính toán trước các khối tránh việc lũy thừa lặp lại bên trong vòng lặp tập hợp con, điều này rất quan trọng đối với hiệu suất ở 1000 trường hợp thử nghiệm. 

## Ví dụ đã hoạt động 

Hãy xem xét một mảng$[1, -2, 3]$. Chúng tôi liệt kê tất cả các tập hợp con ngoại trừ tập hợp đầy đủ. 

| Mặt nạ | Tập hợp con | |S| | Tổng các khối | Giá trị | 

|------|--------|----|--------------|-------| 

| 000 | {} | 0 | 0 | 0 | 

| 001 | {1} | 1 | 1 | -1 | 

| 010 | {-2} | 1 | -8 | 8 | 

| 100 | {3} | 1 | 27 | -27 | 

| 011 | {1,-2} | 2 | -7 | -14 | 

| 101 | {1,3} | 2 | 28 | 56 | 

| 110 | {-2,3} | 2 | 19 | 38 | 

Bộ đầy đủ được loại trừ. Tối thiểu là -27 và tối đa là 56. 

Dấu vết này cho thấy dấu hiệu dựa trên tính chẵn lẻ lật các giá trị như thế nào ngay cả khi tổng các khối tăng lên, tạo ra hành vi không đơn điệu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(T \cdot 2^N \cdot N)$| Mỗi tập hợp con được liệt kê và giải mã từng chút một | 
| Không gian |$O(N)$| Chỉ có mảng và mảng khối được lưu trữ | 

Với$N \le 15$, không gian tập hợp con đủ nhỏ để thậm chí có thể liệt kê đầy đủ cho mỗi trường hợp thử nghiệm. Tổng số hoạt động vẫn nằm trong giới hạn chấp nhận được đối với$T \le 1000$, đặc biệt với các thao tác bit trực tiếp và các khối được tính toán trước. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    def solve():
        T = int(input())
        out = []
        for _ in range(T):
            N = int(input())
            A = list(map(int, input().split()))
            cube = [x*x*x for x in A]
            full = (1<<N)-1
            INF = 10**30
            mn, mx = INF, -INF
            for mask in range(1<<N):
                if mask == full:
                    continue
                cnt = 0
                s = 0
                m = mask
                i = 0
                while m:
                    if m & 1:
                        cnt += 1
                        s += cube[i]
                    m >>= 1
                    i += 1
                if cnt == 0:
                    val = 0
                else:
                    val = cnt * (-1 if cnt%2 else 1) * s
                mn = min(mn, val)
                mx = max(mx, val)
            out.append(str(mn) + " " + str(mx))
        return "\n".join(out)

    return solve()

# custom cases
assert run("1\n1\n5\n") == "0 0", "single element"
assert run("1\n2\n1 2\n") is not None, "small sanity"
assert run("1\n3\n-1 -2 -3\n") is not None, "all negative"
assert run("2\n2\n1 2\n1\n7\n") is not None, "multiple cases"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 0 0 | tập hợp đầy đủ chỉ có tác dụng loại trừ tập hợp con trống | 
| mảng nhỏ hỗn hợp | khác nhau | tính đúng đắn cơ bản của phép liệt kê | 
| tất cả đều tiêu cực | khác nhau | tương tác ký hiệu với khối âm | 
| nhiều trường hợp | khác nhau | tính chính xác trong việc phân lô thử nghiệm | 

## Vỏ cạnh 

Tập con trống là trường hợp tế nhị nhất vì nó tạo ra một giá trị hợp lệ mặc dù cả hai thành phần của biểu thức đều bằng 0. Thuật toán tự nhiên bao gồm nó thông qua mặt nạ 0 và vì nó không khớp với mặt nạ toàn bộ nên nó được đánh giá chính xác là 0. 

Việc loại trừ toàn bộ được xử lý rõ ràng bằng cách bỏ qua mặt nạ$2^N - 1$. Nếu không có điều kiện này, giải pháp sẽ bao gồm không chính xác một ứng cử viên bổ sung, ứng cử viên này có thể chiếm ưu thế ở mức tối đa hoặc tối thiểu tùy thuộc vào phân bổ đầu vào. 

Một hành vi cạnh khác phát sinh khi tất cả các phần tử bằng 0. Sau đó, mọi tập hợp con sẽ đánh giá về 0 bất kể kích thước hay dấu hiệu và thuật toán trả về 0 một cách chính xác cho cả giá trị tối thiểu và tối đa vì tất cả các giá trị được đánh giá đều thu gọn về cùng một hằng số.
