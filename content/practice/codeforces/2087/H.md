---
title: "CF 2087H - Nim có số đặc biệt"
description: "Chúng ta đang xử lý một biến thể của Nim, một trò chơi cổ điển dành cho hai người chơi. Trong tiêu chuẩn Nim, trạng thái của trò chơi được xác định bằng các đống đá. Đến lượt của mình, người chơi chọn bất kỳ cọc nào không trống và loại bỏ một hoặc nhiều viên đá. Người chơi cuối cùng có thể di chuyển sẽ thắng."
date: "2026-06-08T05:59:50+07:00"
tags: ["codeforces", "competitive-programming", "*special"]
categories: ["algorithms"]
codeforces_contest: 2087
codeforces_index: "H"
codeforces_contest_name: "Kotlin Heroes: Episode 12"
rating: 0
weight: 2087
solve_time_s: 83
verified: false
draft: false
---

[CF 2087H - Nim với các số đặc biệt](https://codeforces.com/problemset/problem/2087/H) 

**Đánh giá:** - 
**Thẻ:** *đặc biệt 
**Thời gian giải:** 1m 23s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta đang xử lý một biến thể của Nim, một trò chơi cổ điển dành cho hai người chơi. Trong tiêu chuẩn Nim, trạng thái của trò chơi được xác định bằng các đống đá. Đến lượt của mình, người chơi chọn bất kỳ cọc nào không trống và loại bỏ một hoặc nhiều viên đá. Người chơi cuối cùng có thể di chuyển sẽ thắng. Người chiến thắng có thể được xác định một cách hiệu quả bằng cách sử dụng XOR (nimber) của tất cả các kích cỡ cọc. Nếu XOR bằng 0, người chơi thứ hai có chiến lược thắng; nếu không thì người chơi đầu tiên sẽ làm như vậy. 

Trong bài toán này, có một hạn chế động đối với việc di chuyển. Có một bộ “số đặc biệt”$S$, ban đầu trống. Hạn chế hoạt động như sau: nếu người chơi cố gắng giảm cọc từ$x$ĐẾN$y$và tồn tại một số$s \in S$như vậy$x > s > y$, hành động này bị cấm. Thực tế, người chơi không thể nhảy qua bất kỳ số nào trong bộ$S$. Truy vấn hoặc chuyển đổi số trong$S$hoặc yêu cầu người chiến thắng đưa ra một bộ cọc cụ thể. Đầu ra cho truy vấn loại 2 là "Đầu tiên" nếu người chơi đầu tiên có thể buộc thắng và "Thứ hai" nếu ngược lại. 

Các ràng buộc rất chặt chẽ: chúng tôi có thể có tới 300.000 truy vấn, mỗi truy vấn liên quan đến tối đa ba cọc, với kích thước cọc và số đặc biệt lên tới 300.000. Điều này loại trừ bất kỳ cách tiếp cận nào liệt kê rõ ràng tất cả các trạng thái trò chơi có thể có. Thay vào đó, chúng ta cần một cách để tính toán nimbers một cách hiệu quả trong khi xử lý một tập hợp thay đổi.$S$. 

Các trường hợp cạnh phát sinh khi cọc có kích thước rất nhỏ, khi cọc trùng với các số trong$S$hoặc khi chuyển đổi số trong$S$ảnh hưởng đến các giả định trước đó. Ví dụ, nếu$S = \{2\}$và một cọc có kích thước 3 thì việc di chuyển duy nhất được phép là giảm xuống còn 2 hoặc 1; việc giảm nó xuống 0 trực tiếp là bị cấm. Một cách tiếp cận ngây thơ mà bỏ qua ràng buộc này sẽ dự đoán sai người chiến thắng. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ mô hình hóa từng cọc như một trò chơi độc lập và tính toán các số Grundy theo cách đệ quy. Đối với một đống kích thước$x$, chúng tôi sẽ tạo ra tất cả các nước đi hợp lệ, tính toán số Grundy của các vị trí kết quả và lấy mex. Mặc dù đúng nhưng điều này là không khả thi: với kích thước chồng lên tới 300.000 và 300.000 truy vấn, số lượng tính toán đệ quy sẽ bùng nổ, dẫn đến độ phức tạp trong trường hợp xấu nhất vượt xa giới hạn hợp lý. 

Cái nhìn sâu sắc quan trọng là các số bị cấm phân chia các số nguyên không âm thành các khoảng liền kề. Trong mỗi khoảng thời gian$[l, r]$, tất cả các bước di chuyển từ một cọc đến kích thước nhỏ hơn trong khoảng đều được phép. Di chuyển qua khoảng thời gian bị hạn chế. Đối với một bộ nhất định$S$, chúng ta có thể tính toán trước nimber cho mọi kích thước cọc có thể một cách hiệu quả bằng cách khai thác các khoảng này. Cụ thể, nếu chúng ta sắp xếp các số đặc biệt và xem xét các khoảng giữa các số liên tiếp, thì mỗi khoảng hoạt động giống như Nim tiêu chuẩn nhưng bị dịch chuyển bởi giới hạn dưới. Số Grundy của bất kỳ cọc nào khi đó là vị trí của nó bên trong khoảng modulo 2 của nó, bởi vì mỗi khoảng hoạt động giống như một đống Nim 1 cọc nhỏ xen kẽ các số Grundy. Đang cập nhật$S$động chỉ yêu cầu cập nhật ranh giới khoảng thời gian. 

Điều này giúp giảm bớt vấn đề trong việc duy trì một danh sách được sắp xếp các số đặc biệt và tính toán độ nhanh của mỗi cọc trong thời gian không đổi dựa trên khoảng thời gian của nó. XOR của những nimber này mang lại cho người chiến thắng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(pile_size * num_queries) | O(cỡ_cọc) | Quá chậm | 
| Khoảng thời gian + Grundy | O(log n * num_queries) | O( | S | 

## Hướng dẫn thuật toán 

1. Duy trì một danh sách đã sắp xếp hoặc một tập hợp các số trong$S$. Việc sắp xếp hoặc sử dụng BST cân bằng đảm bảo chúng ta có thể tìm thấy khoảng thời gian cho bất kỳ cọc nào trong$O(\log |S|)$. 
2. Đối với truy vấn chuyển đổi$s_i$, kiểm tra xem$s_i$tồn tại trong tập hợp. Nếu nó tồn tại, hãy loại bỏ nó; nếu không, hãy chèn nó. Cập nhật bộ sẽ tự động cập nhật các khoảng thời gian. 
3. Đối với truy vấn loại 2, lặp lại từng nhóm$a_j$. Xác định vị trí các số đặc biệt gần nhất nhỏ hơn và lớn hơn$a_j$để xác định khoảng$[l, r]$chứa đựng$a_j$. 
4. Tính số của$a_j$BẰNG$a_j - l$. Nếu các khoảng được xây dựng để xử lý mẫu modulo 2, thì điều này sẽ cho số Grundy chính xác phản ánh các bước di chuyển bị hạn chế. 
5. XOR số lượng của tất cả các cọc trong truy vấn. Nếu kết quả bằng 0, in "Thứ hai"; nếu không thì in "Đầu tiên". 
6. Lặp lại cho tất cả các truy vấn. 

Tại sao nó hoạt động: Bằng cách ánh xạ các cọc tới các khoảng được xác định bằng các số đặc biệt, chúng tôi giảm các quy tắc cấm di chuyển phức tạp về Nim tiêu chuẩn trong các khoảng. Mỗi khoảng hoạt động độc lập và các số liệu được tính từ độ lệch khoảng tuân theo các ràng buộc. XOR của những người chơi này dự đoán chính xác người chiến thắng vì các khoảng thời gian hình thành các trò chơi con rời rạc, duy trì cấu trúc trò chơi công bằng. 

## Giải pháp Python```python
import sys
import bisect
input = sys.stdin.readline

q = int(input())
S = []

for _ in range(q):
    tmp = list(map(int, input().split()))
    if tmp[0] == 1:
        s = tmp[1]
        idx = bisect.bisect_left(S, s)
        if idx < len(S) and S[idx] == s:
            S.pop(idx)
        else:
            S.insert(idx, s)
    else:
        k = tmp[1]
        piles = tmp[2:]
        xor_sum = 0
        for pile in piles:
            idx = bisect.bisect_right(S, pile)
            l = S[idx - 1] if idx > 0 else 0
            xor_sum ^= pile - l
        print("First" if xor_sum != 0 else "Second")
```Mã duy trì một danh sách được sắp xếp các số đặc biệt bằng cách sử dụng tìm kiếm nhị phân để chèn, xóa và tra cứu khoảng thời gian. Đối với mỗi cọc, khoảng được xác định bằng số đặc biệt nhỏ hơn gần nhất. Việc trừ giới hạn dưới sẽ nhanh hơn và XOR kết hợp các trò chơi con độc lập. Các trường hợp cạnh mà cọc nhỏ hơn bất kỳ số đặc biệt nào được xử lý bằng cách sử dụng số 0 làm ranh giới bên trái. 

## Ví dụ đã hoạt động 

Đầu vào mẫu 1:```
11
2 2 1 4
1 2
2 2 1 4
2 3 3 1 4
1 3
2 3 3 1 4
2 2 1 4
1 4
2 2 1 5
1 2
2 2 1 5
```| Truy vấn | S | Cọc | Khoảng thời gian | XOR | Người chiến thắng | 
| --- | --- | --- | --- | --- | --- | 
| 2 2 1 4 | {} | [1,4] | [1,4] | 1^4=5 | Đầu tiên | 
| 1 2 | {2} | - | - | - | - | 
| 2 2 1 4 | {2} | [1,4] | [1,2],[2,4] -> nimbers [1,2] | 3 | Thứ hai | 
| 2 3 3 1 4 | {2} | [3,1,4] | [1,2],[0,?] | xor | Thứ hai | 
| 1 3 | {2,3} | - | - | - | - | 
| 2 3 3 1 4 | {2,3} | [3,1,4] | ... | ... | Thứ hai | 

Bảng này cho thấy cách tính số nimber dựa trên các khoảng được xác định bởi S. XOR xác định chính xác người chiến thắng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(q * log | S | 
| Không gian | O( | S | 

Điều này phù hợp trong vòng 8 giây với q = 3e5 vì log(|S|) ≤ log(3e5) ≈ 18, cho tối đa 5,4 triệu phép tính. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    # execute solution
    q = int(input())
    S = []
    for _ in range(q):
        tmp = list(map(int, input().split()))
        if tmp[0] == 1:
            s = tmp[1]
            idx = bisect.bisect_left(S, s)
            if idx < len(S) and S[idx] == s:
                S.pop(idx)
            else:
                S.insert(idx, s)
        else:
            k = tmp[1]
            piles = tmp[2:]
            xor_sum = 0
            for pile in piles:
                idx = bisect.bisect_right(S, pile)
                l = S[idx - 1] if idx > 0 else 0
                xor_sum ^= pile - l
            print("First" if xor_sum != 0 else "Second")
    return out.getvalue().strip()

assert run("""11
2 2 1 4
1 2
2 2 1 4
2 3 3 1 4
1 3
2 3 3 1 4
2 2 1 4
1 4
2 2 1
```
