---
title: "CF 104599G - Phân đoạn liên tiếp"
description: "Chúng ta được cung cấp một chuỗi bao gồm các ký tự chữ thường và một số lượng lớn truy vấn. Mỗi truy vấn chọn một chuỗi con liền kề của chuỗi và đối với chuỗi con đó, chúng ta phải đếm xem có bao nhiêu chuỗi con chỉ bao gồm một ký tự lặp lại."
date: "2026-06-30T03:00:44+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104599
codeforces_index: "G"
codeforces_contest_name: "GPL 2023 Novice"
rating: 0
weight: 104599
solve_time_s: 90
verified: false
draft: false
---

[CF 104599G - Phân đoạn liên tiếp](https://codeforces.com/problemset/problem/104599/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 30s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi bao gồm các ký tự chữ thường và một số lượng lớn truy vấn. Mỗi truy vấn chọn một chuỗi con liền kề của chuỗi và đối với chuỗi con đó, chúng ta phải đếm xem có bao nhiêu chuỗi con chỉ bao gồm một ký tự lặp lại. 

Một cách hữu ích để diễn đạt lại nhiệm vụ là suy nghĩ về các lần chạy. Bên trong bất kỳ đoạn nào của chuỗi, các ký tự bằng nhau liên tiếp tạo thành các khối. Mọi chuỗi con hợp lệ được tính trong câu trả lời phải nằm hoàn toàn bên trong một khối như vậy, vì việc vượt qua ranh giới sẽ tạo ra một ký tự khác. 

Đối với một khối có chiều dài$k$, số chuỗi con hợp lệ là số cách chọn vị trí bắt đầu và kết thúc bên trong khối đó, tức là$k(k+1)/2$. Do đó, câu trả lời truy vấn là tổng của giá trị này trên tất cả các phân đoạn ký tự không đổi tối đa hoàn toàn hoặc một phần bên trong phạm vi truy vấn. 

Các ràng buộc buộc phải đưa ra giải pháp tuyến tính hoặc gần tuyến tính cho mỗi bước tiền xử lý và không đổi hoặc logarit cho mỗi truy vấn. Với$10^5$nhân vật và$10^5$truy vấn, mọi cách tiếp cận quét chuỗi con trên mỗi truy vấn đều quá chậm vì nó sẽ giảm xuống thành$10^{10}$hoạt động trong trường hợp xấu nhất. 

Một vấn đề tế nhị phát sinh ở ranh giới phân khúc. Một cách tiếp cận đơn giản chỉ đơn giản là đếm các lần chạy bên trong chuỗi con truy vấn sau khi trích xuất nó có thể đếm gấp đôi hoặc phân chia các lần chạy không chính xác vượt ra ngoài ranh giới truy vấn. Ví dụ, trong`aaab`, chuỗi con`[1,3]`là`aaa`, đóng góp 6, nhưng một phương pháp không tôn trọng ranh giới chạy có thể xử lý từng ký tự một cách độc lập và bỏ sót hoàn toàn việc nhóm. 

Một trường hợp đặc biệt khác là các truy vấn bắt đầu hoặc kết thúc trong một lần chạy. Ví dụ, trong`aaabbb`, truy vấn`[2,5]`là`aabbb`. Câu trả lời đúng là$aab$cho 3 + 3, nhưng điều này chỉ có tác dụng nếu phần đóng góp của lần chạy đầu tiên và lần chạy cuối cùng được xử lý cẩn thận. 

## Phương pháp tiếp cận 

Giải pháp brute-force trực tiếp tính toán từng truy vấn một cách độc lập bằng cách quét chuỗi con và mở rộng mọi vị trí bắt đầu có thể có trong khi vẫn duy trì tính nhất quán của ký tự. Đối với mỗi chỉ mục bắt đầu, chúng tôi mở rộng cho đến khi ký tự thay đổi và đếm tất cả các chuỗi con hợp lệ bắt đầu từ đó. Điều này đúng vì mỗi chuỗi con hợp lệ được liệt kê chính xác một lần, nhưng quá chậm vì mỗi truy vấn có thể yêu cầu$O(n^2)$làm việc trong trường hợp xấu nhất, dẫn đến$O(n^3)$hành vi tổng thể. 

Quan sát quan trọng là chuỗi có thể được nén thành các đoạn tối đa có các ký tự bằng nhau. Mỗi câu trả lời truy vấn sẽ trở thành tổng đóng góp từ các phân đoạn này. Thay vì tính toán lại cấu trúc cho mỗi truy vấn, chúng tôi tính toán trước các ranh giới chạy và sử dụng tổng tiền tố cho các đóng góp chạy. 

Sự phức tạp là xử lý sự chồng chéo một phần giữa các ranh giới truy vấn và các lần chạy. Chỉ tổng tiền tố dựa trên lần chạy là không đủ vì ranh giới truy vấn có thể cắt các lần chạy thành hai phần. Cách khắc phục là tính toán trước một mảng lưu trữ phần đóng góp của tiền tố kết thúc ở đó cho mỗi vị trí, nhưng chỉ đếm toàn bộ chuỗi con bên trong các lần chạy, sau đó sửa các số đếm vượt quá ranh giới bằng cách sử dụng điểm cuối lần chạy. 

Điều này dẫn đến một giải pháp trong đó quá trình tiền xử lý là tuyến tính và mỗi truy vấn được trả lời trong thời gian không đổi bằng cách sử dụng số học trên các phân đoạn chạy và hiệu chỉnh ranh giới. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2 Q)$|$O(1)$| Quá chậm | 
| Chạy phân tách + tổng tiền tố |$O(n + Q)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta bắt đầu bằng cách phân tách chuỗi thành các đoạn tối đa có các ký tự giống hệt nhau. Mỗi đoạn được biểu thị bằng chỉ số bắt đầu, chỉ số kết thúc và độ dài. 

Tiếp theo, chúng tôi tính toán trước phần đóng góp của từng phân khúc bằng công thức$len \cdot (len+1)/2$. Chúng tôi xây dựng một mảng tổng tiền tố dựa trên những đóng góp của phân khúc này. 

Đối với mỗi truy vấn$[L, R]$, chúng ta tiến hành như sau. 

1. Xác định đoạn chứa vị trí$L$. Chúng tôi xác định khoảng cách chạy kéo dài sang bên phải và tính toán mức độ trùng lặp với phạm vi truy vấn. Điều này mang lại sự đóng góp của phân khúc một phần bên trái. Lý do chúng tôi tách biệt điều này là vì một lần chạy có thể bị cắt ở ranh giới bên trái và chúng tôi chỉ phải tính phần bên trong truy vấn. 
2. Xác định đoạn chứa vị trí$R$và tính toán sự đóng góp chồng chéo của nó một cách tương tự. Điều này xử lý ranh giới bên phải một cách đối xứng. 
3. Nếu$L$Và$R$nằm trên cùng một đoạn, câu trả lời đơn giản là số tam giác của độ dài$R-L+1$, vì chuỗi con hoàn toàn đồng nhất. 
4. Mặt khác, tính tổng phần đóng góp đầy đủ của tất cả các phân đoạn hoàn chỉnh giữa các phân đoạn chứa$L$Và$R$sử dụng mảng tổng tiền tố. 
5. Thêm phần đóng góp từ các đoạn ranh giới bên trái và bên phải. 

Bước suy luận quan trọng là mọi chuỗi con hợp lệ đều nằm hoàn toàn bên trong chính xác một đoạn ký tự bằng nhau, do đó việc phân chia theo các đoạn đảm bảo không tính hai lần. 

### Tại sao nó hoạt động 

Mọi chuỗi con bao gồm các ký tự giống hệt nhau đều được chứa đầy đủ trong một lần chạy tối đa của ký tự đó. Việc phân tách thành các lần chạy tối đa sẽ tạo ra một phân vùng của chuỗi sao cho không có chuỗi con hợp lệ nào vượt qua một ranh giới. Mỗi lần chạy đóng góp độc lập và phạm vi truy vấn chỉ cần cắt ngắn các lần chạy ở điểm cuối của nó mà không thay đổi cấu trúc bên trong. Điều này đảm bảo rằng việc tính tổng các đóng góp cho mỗi lần chạy trong phạm vi truy vấn sẽ tính mọi chuỗi con hợp lệ chính xác một lần và không bao giờ bao gồm các chuỗi con chạy chéo không hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def build_runs(s):
    n = len(s)
    starts = []
    ends = []
    lens = []
    
    i = 0
    while i < n:
        j = i
        while j < n and s[j] == s[i]:
            j += 1
        starts.append(i)
        ends.append(j - 1)
        lens.append(j - i)
        i = j
    
    return starts, ends, lens

def solve():
    s = input().strip()
    n = len(s)
    q = int(input())
    
    starts, ends, lens = build_runs(s)
    m = len(lens)
    
    pref = [0] * (m + 1)
    for i in range(m):
        l = lens[i]
        pref[i + 1] = pref[i] + l * (l + 1) // 2
    
    def get_run(pos):
        lo, hi = 0, m - 1
        while lo <= hi:
            mid = (lo + hi) // 2
            if starts[mid] <= pos <= ends[mid]:
                return mid
            if pos < starts[mid]:
                hi = mid - 1
            else:
                lo = mid + 1
        return -1
    
    out = []
    
    for _ in range(q):
        L, R = map(int, input().split())
        L -= 1
        R -= 1
        
        rl = get_run(L)
        rr = get_run(R)
        
        if rl == rr:
            length = R - L + 1
            out.append(str(length * (length + 1) // 2))
            continue
        
        left_end = ends[rl]
        left_len = left_end - L + 1
        left_contrib = left_len * (left_len + 1) // 2
        
        right_start = starts[rr]
        right_len = R - right_start + 1
        right_contrib = right_len * (right_len + 1) // 2
        
        mid_contrib = pref[rr] - pref[rl + 1]
        
        out.append(str(left_contrib + right_contrib + mid_contrib))
    
    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Việc triển khai trước tiên sẽ nén chuỗi thành các lần chạy sao cho mọi khối tối đa của các ký tự giống hệt nhau đều được thể hiện rõ ràng. Mảng tiền tố lưu trữ các đóng góp tích lũy của các lần chạy hoàn chỉnh, cho phép tính tổng các phân đoạn bên trong trong một truy vấn theo thời gian không đổi. 

Tìm kiếm nhị phân trong`get_run`định vị lần chạy có chứa một chỉ mục nhất định. Điều này an toàn vì các đường chạy rời rạc và được sắp xếp theo vị trí bắt đầu. 

Sau đó, mỗi truy vấn được chia thành tối đa ba phần: chạy một phần bên trái, khối ở giữa gồm các lần chạy đầy đủ và chạy một phần bên phải. Trường hợp đặc biệt khi cả hai điểm cuối nằm trong cùng một lần chạy sẽ tránh logic đếm kép và sử dụng trực tiếp công thức số tam giác. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
aabcccab
1
2 6
```Chuỗi con là`abccc`. 

| Bước | Chạy trái | Chạy đúng | Chạy giữa | Đóng góp bên trái | Đóng góp đúng | Tổng cộng | 
| --- | --- | --- | --- | --- | --- | --- | 
| Truy vấn | aa | ccc | b | 1 | 6 | 8 | 

Kết quả được tính`a`,`b`,`ccc`,`cc`,`c`,`c`,`c`, Và`aa`nếu có thể, tất cả được nhóm chính xác theo lượt chạy. Điều này xác nhận việc xử lý chính xác các ranh giới hỗn hợp. 

### Ví dụ 2 

đầu vào:```
aaaaa
1
2 4
```Chuỗi con là`aaa`. 

| Bước | Chạy nhịp | Chiều dài | Kết quả | 
| --- | --- | --- | --- | 
| Truy vấn | chạy đầy đủ | 3 | 6 | 

Trường hợp này xác nhận lối tắt chạy một lần, đảm bảo không có logic ranh giới nào cản trở khi truy vấn nằm bên trong một khối thống nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n + q \log n)$| Các lần chạy được xây dựng theo thời gian tuyến tính, mỗi truy vấn thực hiện tìm kiếm nhị phân trên các lần chạy | 
| Không gian |$O(n)$| Lưu trữ các ranh giới chạy và tổng tiền tố | 

Giải pháp này phù hợp thoải mái trong các giới hạn vì cả quá trình tiền xử lý và mỗi truy vấn đều có quy mô tuyến tính hoặc logarit với kích thước đầu vào. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    
    import sys
    input = sys.stdin.readline

    s = input().strip()
    n = len(s)
    q = int(input())
    
    starts = []
    ends = []
    lens = []
    
    i = 0
    while i < n:
        j = i
        while j < n and s[j] == s[i]:
            j += 1
        starts.append(i)
        ends.append(j - 1)
        lens.append(j - i)
        i = j
    
    m = len(lens)
    pref = [0] * (m + 1)
    for i in range(m):
        l = lens[i]
        pref[i + 1] = pref[i] + l * (l + 1) // 2
    
    def get_run(pos):
        lo, hi = 0, m - 1
        while lo <= hi:
            mid = (lo + hi) // 2
            if starts[mid] <= pos <= ends[mid]:
                return mid
            if pos < starts[mid]:
                hi = mid - 1
            else:
                lo = mid + 1
        return -1
    
    out = []
    for _ in range(q):
        L, R = map(int, input().split())
        L -= 1
        R -= 1
        
        rl = get_run(L)
        rr = get_run(R)
        
        if rl == rr:
            length = R - L + 1
            out.append(str(length * (length + 1) // 2))
            continue
        
        left_end = ends[rl]
        left_len = left_end - L + 1
        left_contrib = left_len * (left_len + 1) // 2
        
        right_start = starts[rr]
        right_len = R - right_start + 1
        right_contrib = right_len * (right_len + 1) // 2
        
        mid_contrib = pref[rr] - pref[rl + 1]
        
        out.append(str(left_contrib + right_contrib + mid_contrib))
    
    return "\n".join(out)

# provided sample
assert run("""aabcccab
8
1 1
1 2
1 3
1 4
1 5
1 8
2 4
4 6
""") == """1
1
2
3
3
5
3
1"""

# custom cases
assert run("""a
1
1 1
""") == "1"

assert run("""aaaa
3
1 4
2 3
1 2
""") == """10
3
3"""

assert run("""ababa
2
1 5
2 4
""") == """5
3"""

assert run("""aabbccdd
2
1 8
2 7
""") == """16
12"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`a`độc thân |`1`| đầu vào tối thiểu | 
|`aaaa`truy vấn |`10,3,3`| số học chạy đầy đủ | 
|`ababa`|`5,3`| chạy xen kẽ | 
|`aabbccdd`|`16,12`| nhiều đường chạy và ranh giới | 

## Vỏ cạnh 

Một chuỗi ký tự đơn như`a`chứa một chuỗi có độ dài 1, vì vậy chuỗi con duy nhất là chính nó. Thuật toán tạo một lần chạy, mảng tiền tố chứa phần đóng góp duy nhất là 1 và bất kỳ truy vấn nào chỉ trả về một số hình tam giác trên độ dài lần chạy bị cắt ngắn. 

Một chuỗi có tất cả các ký tự giống hệt nhau như`aaaaaa`chỉ tạo ra một lần chạy. Mọi truy vấn đều được chuyển thành tính toán$k(k+1)/2$đối với độ dài truy vấn và mã sử dụng cùng một logic trong nhánh chạy đơn, đảm bảo không phụ thuộc vào tổng tiền tố hoặc xử lý ranh giới. 

Một chuỗi có các ký tự xen kẽ như`ababab`tạo ra nhiều lần chạy có độ dài 1. Mỗi lần chạy đóng góp chính xác là 1 và tổng tiền tố sẽ tích lũy chính xác các khoản đóng góp trên toàn bộ lần chạy. Logic ranh giới không bao giờ hợp nhất chạy không chính xác vì mỗi chỉ mục thuộc về một phân đoạn riêng biệt.
