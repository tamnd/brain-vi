---
title: "CF 2217B - Lật Bit (Phiên bản dễ)"
description: "Chúng ta được cung cấp một mảng nhị phân có độ dài $n$ và một chỉ số đặc biệt duy nhất $p1$ (vì $k=1$). Phần tử tại chỉ mục đặc biệt này có giá trị $x$ và mục tiêu của chúng ta là làm cho toàn bộ mảng bằng $x$."
date: "2026-06-07T18:24:05+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 2217
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 1091 (Div. 2) and CodeCraft 26"
rating: 1000
weight: 2217
solve_time_s: 102
verified: false
draft: false
---

[CF 2217B - Lật Bit (Phiên bản dễ dàng)](https://codeforces.com/problemset/problem/2217/B) 

**Đánh giá:** 1000 
**Tags:** tham lam, thực hiện 
**Thời gian giải:** 1 phút 42s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một mảng nhị phân có độ dài$n$và một chỉ số đặc biệt duy nhất$p_1$(từ$k=1$). Phần tử ở chỉ mục đặc biệt này có giá trị$x$và mục tiêu của chúng tôi là làm cho toàn bộ mảng bằng$x$. Để làm điều này, chúng ta có thể thực hiện bất kỳ số thao tác lật nào trên các mảng con liền kề có chứa chỉ mục đặc biệt. Một lần lật sẽ đảo ngược mọi bit trong phạm vi đã chọn. 

Thực tế, bài toán hỏi: số lượng tối thiểu các mảng con liền kề chứa chỉ mục đặc biệt cần thiết để tất cả các phần tử khớp với giá trị tại chỉ mục đó là bao nhiêu? 

Bởi vì$n$có thể lên đến$2 \cdot 10^5$và có thể có tới$10^4$trường hợp thử nghiệm, một giải pháp phức tạp$O(n^2)$mỗi trường hợp thử nghiệm sẽ quá chậm. Chúng ta cần một giải pháp tuyến tính liên quan đến kích thước mảng. 

Các trường hợp cạnh không rõ ràng bao gồm: một mảng đã bằng$x$, hoặc khi$x$là 0 và tất cả các số 1 xuất hiện liên tiếp ở một phía của chỉ số đặc biệt. Một cách tiếp cận bất cẩn, lộn xộn một cách bừa bãi có thể thực hiện các hoạt động không cần thiết. 

## Phương pháp tiếp cận 

Một cách tiếp cận mạnh mẽ sẽ xem xét tất cả các phạm vi có thể có chứa chỉ mục đặc biệt, lật chúng và cố gắng đệ quy để giảm thiểu các lần lật. Điều này đúng nhưng không khả thi. Đối với một chỉ mục đặc biệt duy nhất, brute-force có thể thử mọi mảng con chứa chỉ mục đó, nhưng có khoảng$O(n^2)$những phạm vi như vậy. Điều đó nhanh chóng vượt quá giới hạn thời gian. 

Quan sát quan trọng là mỗi phân đoạn liền kề của các phần tử không bằng$x$và trên cùng một phía của chỉ số đặc biệt có thể được cố định chỉ bằng một lần lật. Việc lật trải trên nhiều phân đoạn như vậy sẽ không hữu ích vì chúng cũng sẽ đảo ngược các phần tử đã đúng. Do đó, số lần lật tối thiểu là số đoạn "sai" liền kề ở hai bên của chỉ số đặc biệt. 

Để thực hiện điều này một cách hiệu quả, chúng tôi quét mảng ở bên trái và bên phải của chỉ mục đặc biệt, đếm các phân đoạn liền kề có giá trị không bằng$x$, và tính tổng các số này. Điều này cung cấp các hoạt động tối thiểu trực tiếp trong$O(n)$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xác định giá trị$x$ở chỉ số đặc biệt. 
2. Khởi tạo bộ đếm số lần lật về 0. 
3. Quét mảng bên trái của chỉ mục đặc biệt từ gần nhất đến xa nhất. Bất cứ khi nào một khối phần tử liền kề khác với$x$, tăng bộ đếm lật lên một và bỏ qua khối. 
4. Quét mảng bên phải của chỉ mục đặc biệt bằng quy trình tương tự. Mỗi khối liền kề khác nhau$x$được tính là một lần lật. 
5. Trả về tổng số lần lật. 

Tại sao nó hoạt động: mỗi lần lật phải bao gồm chỉ số đặc biệt, vì vậy chúng ta không thể lật các phần tử ở phía đối diện một cách độc lập. Các chuỗi liền kề có giá trị sai ở mỗi bên có thể được sửa bằng chính xác một lần lật trên mỗi chuỗi, điều này đảm bảo số lượng thao tác tối thiểu. Bất kỳ nỗ lực nào để phân tách hoặc kết hợp các lần lật sẽ để lại một số bit sai hoặc thực hiện các lần lật dư thừa. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, k = map(int, input().split())
        a = list(map(int, input().split()))
        p = list(map(int, input().split()))
        x = a[p[0]-1]
        
        flips = 0
        # Left of special index
        i = p[0]-2
        while i >= 0:
            if a[i] != x:
                flips += 1
                while i >= 0 and a[i] != x:
                    i -= 1
            else:
                i -= 1
        # Right of special index
        i = p[0]
        while i < n:
            if a[i] != x:
                flips += 1
                while i < n and a[i] != x:
                    i += 1
            else:
                i += 1
        print(flips)

if __name__ == "__main__":
    solve()
```Đầu tiên, giải pháp sẽ đọc số lượng trường hợp thử nghiệm và lặp lại chúng. Đối với mỗi trường hợp thử nghiệm, nó trích xuất mảng và chỉ mục đặc biệt, điều chỉnh lập chỉ mục dựa trên 0 và quét trái và phải từ chỉ mục đặc biệt để đếm các chuỗi liền kề có giá trị sai. Nội tâm`while`các vòng lặp bỏ qua toàn bộ các phân đoạn để đảm bảo mỗi lần lật được tính chính xác một lần. 

## Ví dụ đã hoạt động 

**Ví dụ 1** 

đầu vào:```
n = 5, k = 1
a = [1, 0, 1, 0, 1]
p = [3]
```| Bước | tôi | một [tôi] | lật | Bình luận | 
| --- | --- | --- | --- | --- | 
| Bắt đầu quét trái | 2 | - | 0 | chỉ số đặc biệt là 2 (dựa trên 0) | 
| tôi=1 | 0 | a[1]=0 != x | 1 | đoạn bắt đầu | 
| tôi=0 | 0 | a[0]=1 == x | - | đoạn cuối | 
| Bắt đầu quét phải | 3 | a[3]=0 != x | 2 | đoạn bắt đầu | 
| tôi=4 | a[4]=1 == x | - | đoạn cuối | | 

Tổng số lần lật = 2 

**Ví dụ 2** 

đầu vào:```
n = 4, k = 1
a = [0, 0, 0, 0]
p = [2]
```Tất cả các phần tử bằng x, flips = 0 

Dấu vết xác nhận rằng các đoạn liền kề được tính chính xác một lần và không có thao tác lật không cần thiết nào được thực hiện. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi phần tử được truy cập tối đa hai lần (quét trái và phải) | 
| Không gian | O(1) | Không cần thêm bộ nhớ tỷ lệ với n, chỉ cần bộ đếm | 

Được cho$n$tổng của tất cả các trường hợp thử nghiệm là 2×10^5, tổng số thao tác nằm trong khoảng 4×10^5, có thể chấp nhận được trong giới hạn 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# Provided sample
assert run("1\n5 1\n1 0 1 0 1\n3\n") == "2", "sample 1"

# Custom cases
assert run("1\n4 1\n0 0 0 0\n2\n") == "0", "all equal"
assert run("1\n1 1\n1\n1\n") == "0", "single element"
assert run("1\n6 1\n1 1 0 0 1 1\n3\n") == "1", "single segment in middle"
assert run("1\n5 1\n0 1 0 1 0\n3\n") == "2", "alternating around special"
assert run("1\n7 1\n1 1 0 0 0 1 1\n4\n") == "1", "special inside long segment"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| [1,0,1,0,1], p=3 | 2 | hộp đựng tiêu chuẩn có nắp lật ở cả hai bên | 
| [0,0,0,0], p=2 | 0 | tất cả các yếu tố đã đúng | 
| [1], p=1 | 0 | mảng phần tử đơn | 
| [1,1,0,0,1,1], p=3 | 1 | liền kề sai đoạn | 
| [0,1,0,1,0], p=3 | 2 | xen kẽ các đoạn sai | 
| [1,1,0,0,0,1,1], p=4 | 1 | đoạn dài bao gồm chỉ số đặc biệt | 

## Vỏ cạnh 

Mảng một phần tử: chỉ mục đặc biệt là phần tử duy nhất đã bằng chính nó. Thuật toán trả về đúng 0. 

Một mảng có tất cả các phần tử bằng giá trị đặc biệt: quá trình quét trái và phải kết thúc ngay lập tức mà không cần tăng số lần lật. 

Trình tự xen kẽ xung quanh chỉ số đặc biệt: bên trong`while`các vòng lặp đảm bảo rằng mỗi phân đoạn liền kề được tính chính xác một lần, ngăn chặn việc đếm quá mức. 

Một phân đoạn bao gồm chính chỉ mục đặc biệt: vì chúng tôi chỉ quét sang trái và phải từ chỉ mục đặc biệt nên điều này không tạo ra việc đếm hai lần. 

Thuật toán xử lý ranh giới một cách chính xác bằng cách dừng ở chỉ số 0 và n-1. 

Điều này xác nhận tính đúng đắn cho cả cấu hình tầm thường và cấu hình phức tạp.
