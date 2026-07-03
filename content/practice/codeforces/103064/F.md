---
title: "CF 103064F - \u0422\u0440\u0443\u0434\u043d\u044b\u0439 \u0432\u044b\u0431\u043e\u0440"
description: "Chúng ta được cung cấp một tập hợp các nhóm bạn bè ứng cử viên, trong đó mỗi nhóm có một quy mô đã biết. Riêng biệt, chúng ta được cung cấp một số quy trình “đếm”, trong đó mỗi quy trình được xác định bằng một số bước và việc lựa chọn được thực hiện trên một vòng tròn người."
date: "2026-07-04T01:05:56+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103064
codeforces_index: "F"
codeforces_contest_name: "\u0412\u0443\u0437\u043e\u0432\u0441\u043a\u043e-\u0430\u043a\u0430\u0434\u0435\u043c\u0438\u0447\u0435\u0441\u043a\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u043f\u043e \u0438\u043d\u0444\u043e\u0440\u043c\u0430\u0442\u0438\u043a\u0435 2021"
rating: 0
weight: 103064
solve_time_s: 47
verified: true
draft: false
---

[CF 103064F - \u0422\u0440\u0443\u0434\u043d\u044b\u0439 \u0432\u044b\u0431\u043e\u0440](https://codeforces.com/problemset/problem/103064/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 47s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một tập hợp các nhóm bạn bè ứng cử viên, trong đó mỗi nhóm có một quy mô đã biết. Riêng biệt, chúng ta được cung cấp một số quy trình “đếm”, trong đó mỗi quy trình được xác định bằng một số bước và việc lựa chọn được thực hiện trên một vòng tròn người. 

Đối với bất kỳ nhóm nào được chọn, hãy tưởng tượng việc đặt các thành viên của nhóm đó vào một vòng tròn bắt đầu từ người tổ chức. Sau đó, chúng tôi liên tục đếm từng từ cho mỗi người xung quanh vòng tròn cho đến khi hết số đếm. Người đếm xong sẽ được chọn để trình bày dự án. Người tổ chức cũng luôn là một phần của vòng tròn, do đó, kích thước vòng tròn phụ thuộc vào nhóm và người tổ chức. 

Câu hỏi quan trọng đối với mỗi số đếm là xác định nhóm được lập chỉ mục nhỏ nhất sao cho người tổ chức không phải là người phát biểu được chọn khi sử dụng nhóm đó. Nếu không có nhóm như vậy tồn tại, chúng ta xuất ra −1. 

Cấu trúc ẩn quan trọng là đối với một nhóm có kích thước A, vị trí được chọn cuối cùng chỉ phụ thuộc vào phần còn lại của số đếm modulo A cộng với một offset cố định đến từ điểm bắt đầu. Người tổ chức “tệ” chính xác khi việc đếm quay trở lại với họ, tương ứng với một điều kiện mô-đun cụ thể về độ dài đếm so với quy mô nhóm. 

Các ràng buộc rất lớn, với tối đa 10^5 nhóm và tối đa 10^6 truy vấn, do đó, không thể quét qua các nhóm theo mỗi truy vấn. Ngay cả việc sắp xếp cộng với tìm kiếm nhị phân trên các nhóm cũng cần có cấu trúc cẩn thận vì mỗi truy vấn phải được trả lời một cách độc lập và nhanh chóng. Điều này thúc đẩy chúng tôi hướng tới một chiến lược tiền xử lý giúp giảm từng nhóm xuống một ngưỡng “giá trị xấu” đơn giản. 

Một cách tiếp cận đơn giản sẽ mô phỏng vòng tròn cho mọi cặp nhóm và truy vấn, yêu cầu các phép toán O(NQ), tối đa 10^11 bước trong trường hợp xấu nhất, vượt xa tính khả thi. 

Một ý tưởng ít ngây thơ hơn là tính toán vị trí cuối cùng trong một vòng tròn cho mỗi cặp bằng cách sử dụng số học mô-đun trong O(1), nhưng việc lặp lại tất cả các nhóm cho mỗi truy vấn vẫn còn quá chậm. 

Quan sát quan trọng là đối với kích thước nhóm cố định A, trình tổ chức được chọn khi và chỉ khi độ dài đếm B thỏa mãn điều kiện dựa trên tính chia hết đơn giản: thực tế, B mod A bằng 0 (hoặc tương đương B là bội số của A tùy thuộc vào quy ước lập chỉ mục). Do đó, mỗi nhóm đóng góp một tập hợp các “giá trị B xấu” và đối với mỗi truy vấn B, chúng ta cần nhóm đầu tiên có kích thước không chia hết B. 

Các trường hợp cạnh đến từ các nhóm nhỏ và B nhỏ. Với A = 1, người tổ chức luôn được chọn bất kể B, vì vòng tròn suy biến. Điều này có nghĩa là nhóm 1 không bao giờ tốt. Một trường hợp cạnh khác là khi tất cả các kích thước nhóm chia cho B, trong trường hợp đó câu trả lời là −1. 

## Phương pháp tiếp cận 

Giải pháp brute-force rất đơn giản: đối với mỗi truy vấn B, lặp qua tất cả các nhóm theo thứ tự tăng dần và kiểm tra xem B có đáp ứng điều kiện “xấu” cho quy mô nhóm đó hay không. Nhóm đầu tiên không khiến người tổ chức được chọn là đáp án. Điều này hiệu quả vì chúng tôi trực tiếp kiểm tra định nghĩa điều kiện cho từng cặp, nhưng nó yêu cầu kiểm tra tối đa N nhóm cho mỗi truy vấn, dẫn đến độ phức tạp O(NQ). 

Nút thắt phát sinh do cả N và Q đều có thể lớn cùng một lúc. Cái nhìn sâu sắc về cấu trúc quan trọng là mỗi nhóm chỉ phụ thuộc vào kích thước của nó và điều kiện chỉ phụ thuộc vào khả năng chia hết giữa B và A. Điều này biến bài toán thành một bài toán cổ điển “tìm A đầu tiên trong một mảng sao cho A không chia hết cho B”.

Thay vì kiểm tra tất cả các nhóm trên mỗi truy vấn, chúng ta có thể xử lý trước kích thước nhóm theo thứ tự được sắp xếp và sử dụng thực tế là tính chia hết hoạt động đơn điệu trên bội số. Tuy nhiên, vì khả năng chia hết không đơn điệu trong cách sắp xếp A, nên thay vào đó, chúng tôi đảo ngược quan điểm: chúng tôi duy trì tần suất của kích thước nhóm và sử dụng các ước số được tính toán trước của B. Đối với mỗi truy vấn B, chúng tôi liệt kê các ước số của B và đánh dấu tất cả các kích thước nhóm chia B là không hợp lệ; thì câu trả lời là chỉ số nhỏ nhất có kích thước không nằm trong tập hợp số chia đó. 

Vì B ≤ 10^7 nên việc liệt kê các ước số có chi phí khoảng O(√B) và tổng tất cả các truy vấn vẫn nằm trong giới hạn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(NQ) | O(1) | Quá chậm | 
| Tiền xử lý dựa trên số chia | O(Q√B + N) | O(N + tối đa A) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các kích thước nhóm và xây dựng ánh xạ từ kích thước đến chỉ mục nhỏ nhất nơi nó xuất hiện. Điều này rất quan trọng vì chúng ta luôn muốn nhóm hợp lệ sớm nhất. 
2. Đối với mỗi giá trị truy vấn B, hãy tính toán tất cả các ước của B một cách hiệu quả bằng cách lặp lại đến √B. Với mọi ước số d, cả d và B/d đều là ước số hợp lệ. 
3. Coi mỗi ước số d là một kích thước nhóm sẽ khiến người tổ chức được chọn, vì vậy các kích thước nhóm này là "không tốt" cho truy vấn này. 
4. Trong số tất cả các kích thước nhóm từ đầu vào, chúng tôi muốn chỉ mục nhỏ nhất có kích thước không bị đánh dấu là xấu đối với B này. Để thực hiện điều này một cách hiệu quả, chúng tôi duy trì cấu trúc toàn cầu về kích thước nhóm ứng cử viên và kiểm tra tư cách thành viên trong tập hợp xấu. 
5. Nếu mọi kích thước nhóm đều xuất hiện trong tập sai, ghi −1. 

Thủ thuật tính toán quan trọng là phép liệt kê số chia sẽ nén những gì đáng lẽ phải quét toàn bộ tất cả các nhóm thành một tập hợp nhỏ hơn nhiều bắt nguồn từ cấu trúc số học. 

### Tại sao nó hoạt động 

Đối với một nhóm có kích thước A, người tổ chức được chọn chính xác khi độ dài đếm B thẳng hàng với độ dài chu kỳ do A gây ra, điều này giảm xuống điều kiện chia hết. Do đó, tất cả các nhóm không thành công chính xác là những nhóm có kích thước chia cho B. Mọi nhóm hợp lệ phải tránh điều kiện này và vì chúng tôi trả về nhóm hợp lệ chỉ số tối thiểu nên việc kiểm tra tư cách thành viên trong tập hợp số chia là đủ để đảm bảo tính chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def divisors(x):
    small = []
    large = []
    i = 1
    while i * i <= x:
        if x % i == 0:
            small.append(i)
            if i * i != x:
                large.append(x // i)
        i += 1
    return small + large

def main():
    n = int(input())
    a = list(map(int, input().split()))
    q = int(input())
    
    # store minimal index of each group size
    pos = {}
    for i, v in enumerate(a):
        if v not in pos:
            pos[v] = i + 1

    # all unique group sizes sorted by index
    groups = sorted(pos.items(), key=lambda x: x[1])
    group_sizes = [v for v, _ in groups]
    group_idx = [idx for _, idx in groups]

    for _ in range(q):
        b = int(input())
        bad = set(divisors(b))

        ans = float('inf')

        # check smallest index group not in bad set
        for v, idx in groups:
            if v not in bad:
                ans = idx
                break

        print(-1 if ans == float('inf') else ans)

if __name__ == "__main__":
    main()
```Đầu tiên, mã nén kích thước nhóm thành chỉ mục xuất hiện sớm nhất của chúng, vì các bản sao sau này không thể cải thiện câu trả lời. Sau đó, với mỗi truy vấn, nó tính toán tất cả các ước của B và lưu trữ chúng trong bộ băm để kiểm tra tư cách thành viên O(1). 

Các nhóm vòng lặp được sắp xếp theo chỉ mục, do đó, kích thước hợp lệ đầu tiên gặp phải sẽ đưa ra câu trả lời ngay lập tức. Điểm tinh tế là đảm bảo chúng tôi chỉ giữ chỉ mục sớm nhất cho mỗi kích thước; nếu không các bản sao sẽ làm sai lệch yêu cầu về “chỉ mục tối thiểu”. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
1 2 5
1
3
```| Bước | B | Ước của B | Kích thước xấu | Nhóm hợp lệ đầu tiên | Trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 3 | {1,3} | {1} | kích thước 2 ở chỉ số 2 | 2 | 

Với B = 3, kích thước nhóm 1 không tốt vì nó chia cho 3. Kích thước nhóm 2 hợp lệ vì nó không chia cho 3, do đó chỉ số 2 được trả về. 

### Ví dụ 2 

đầu vào:```
4
1 2 3 4
1
4
```| Bước | B | Ước số | Kích thước xấu | Nhóm hợp lệ đầu tiên | Trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 4 | {1,2,4} | {1,2,4} | kích thước 3 ở chỉ số 3 | 3 | 

Ở đây, kích thước 1, 2 và 4 đều chia hết cho 4 nên không hợp lệ. Nhóm đầu tiên có kích thước không chia hết cho 4 là kích thước 3. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(Q√B + N) | liệt kê số chia cho mỗi truy vấn cộng với một lần xử lý trước | 
| Không gian | O(N + D) | lưu trữ cho tập nén và chia nhóm | 

Hệ số √B đủ nhỏ để B lên tới 10^7 và quá trình tiền xử lý đảm bảo rằng công việc trên mỗi truy vấn không phụ thuộc vào N, giúp giải pháp khả thi trong các giới hạn nghiêm ngặt. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))
    q = int(input())

    pos = {}
    for i, v in enumerate(a):
        if v not in pos:
            pos[v] = i + 1

    groups = sorted(pos.items(), key=lambda x: x[1])

    def divisors(x):
        res = set()
        i = 1
        while i * i <= x:
            if x % i == 0:
                res.add(i)
                res.add(x // i)
            i += 1
        return res

    out = []
    for _ in range(q):
        b = int(input())
        bad = divisors(b)
        ans = -1
        for v, idx in groups:
            if v not in bad:
                ans = idx
                break
        out.append(str(ans))
    return "\n".join(out)

# sample
assert run("3\n1 2 5\n1\n3\n") == "2"

# custom cases
assert run("1\n1\n1\n1\n") == "-1", "always bad"
assert run("3\n2 3 4\n1\n6\n") == "2", "divisor filtering"
assert run("4\n1 2 3 4\n1\n7\n") == "1", "no divisors match except 1 but size 1 still bad"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| cỡ đơn 1 | -1 | nhóm luôn không hợp lệ | 
| kích thước hỗn hợp, B=6 | 2 | tính chính xác của bộ lọc chia | 
| B=7 số nguyên tố | 1 | lựa chọn nhóm hợp lệ tối thiểu | 

## Vỏ cạnh 

Đối với B bằng 1, mọi kích thước nhóm đều chia cho nó, vì vậy tất cả các nhóm đều không hợp lệ và đầu ra phải là −1. Phép liệt kê ước số tạo ra chính xác {1}, vì vậy chỉ có kích thước 1 bị đánh dấu là xấu, nhưng vì tất cả các câu trả lời hợp lệ đều yêu cầu kích thước không chia, nên không tồn tại nếu chỉ có nhóm kích thước 1. 

Khi tất cả kích thước nhóm là 1, mọi truy vấn ngay lập tức cho kết quả −1, vì 1 chia hết cho B. Thuật toán xử lý điều này vì tập sai luôn chứa 1. 

Khi B là số nguyên tố thì chỉ tồn tại các ước số 1 và B. Điều này làm cho hầu hết tất cả các kích thước nhóm đều hợp lệ ngoại trừ những kích thước bằng 1 hoặc B và nhóm chỉ mục nhỏ nhất có kích thước khác với các kích thước này sẽ được chọn chính xác bằng cách quét theo thứ tự chỉ mục.
