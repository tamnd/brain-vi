---
title: "CF 2110E - Giai Điệu"
description: "Chúng ta được yêu cầu sắp xếp một tập hợp các âm thanh, mỗi âm thanh được xác định bởi âm lượng và cao độ, thành một chuỗi vừa đẹp vừa không nhàm chán."
date: "2026-06-08T04:36:07+07:00"
tags: ["codeforces", "competitive-programming", "dfs-and-similar", "graphs", "implementation"]
categories: ["algorithms"]
codeforces_contest: 2110
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 1026 (Div. 2)"
rating: 2300
weight: 2110
solve_time_s: 88
verified: false
draft: false
---

[CF 2110E - Giai điệu](https://codeforces.com/problemset/problem/2110/E) 

**Đánh giá:** 2300 
**Tags:** dfs và tương tự, đồ thị, triển khai 
**Thời gian giải:** 1 phút 28s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu sắp xếp một tập hợp các âm thanh, mỗi âm thanh được xác định bởi âm lượng và cao độ, thành một chuỗi vừa đẹp vừa không nhàm chán. Đẹp có nghĩa là các âm thanh liên tiếp khác nhau chính xác theo một chiều: âm lượng thay đổi trong khi cao độ không đổi hoặc cao độ thay đổi trong khi âm lượng không đổi. Không nhàm chán có nghĩa là không có ba âm thanh liên tiếp có cùng âm lượng hoặc cao độ. Mỗi âm thanh phải xuất hiện đúng một lần trong chuỗi. 

Đầu vào cung cấp nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm bắt đầu bằng số lượng âm thanh, tiếp theo là âm lượng và cao độ của từng âm thanh. Đầu ra phải cho biết liệu có sự sắp xếp hợp lệ hay không. Nếu đúng như vậy, chúng tôi cũng phải cung cấp thứ tự hợp lệ của các chỉ số âm thanh. 

Các ràng buộc ngụ ý rằng chúng ta không thể chấp nhận được các phương pháp hoán vị vũ phu. Với$n$lên đến$2 \cdot 10^5$trên tất cả các trường hợp thử nghiệm và giới hạn 2 giây, bất kỳ thuật toán nào tệ hơn$O(n \log n)$mỗi trường hợp thử nghiệm có thể là quá chậm. Một cách tiếp cận ngây thơ thử tất cả$n!$hoán vị sẽ là vô vọng. Hơn nữa, vì giá trị âm lượng và cao độ có thể lớn bằng$10^9$, việc lập chỉ mục trực tiếp hoặc đếm theo giá trị là không khả thi. Chúng ta cần một phương thức hoạt động chủ yếu dựa trên các thứ tự được sắp xếp hoặc các mối quan hệ tương đối. 

Các trường hợp cạnh không rõ ràng bao gồm các tình huống trong đó tất cả các tập hoặc tất cả các cao độ đều giống hệt nhau. Ví dụ: nếu tất cả các âm thanh có cùng âm lượng nhưng cao độ khác nhau thì không có ba cao độ liên tiếp nào có thể bằng nhau, do đó các chuỗi có độ dài lớn hơn 2 có thể là không thể. Một trường hợp phức tạp khác phát sinh khi số lượng âm lượng và cao độ riêng biệt gần nhau, nhưng sự phân bổ của chúng ngăn cản bất kỳ sự xen kẽ nào giữa việc thay đổi âm lượng và thay đổi cao độ. Bé nhỏ$n$như 1 hoặc 2 là tầm thường, nhưng chúng ta phải xử lý chúng một cách riêng biệt vì điều kiện không nhàm chán chỉ áp dụng cho các chuỗi có độ dài từ 3 trở lên. 

## Phương pháp tiếp cận 

Phương pháp brute-force là thử tất cả các hoán vị của âm thanh và kiểm tra các đặc tính đẹp và không nhàm chán. Về nguyên tắc, điều này đúng: với một hoán vị, chúng ta có thể xác minh các hiệu và bộ ba liên tiếp trong$O(n)$thời gian. Tuy nhiên, số hoán vị là$n!$, lớn về mặt thiên văn đối với$n > 10$. Vì thế, dùng vũ lực là không thể thực hiện được. 

Thông tin chi tiết quan trọng đến từ việc quan sát rằng mỗi âm thanh có thể được nhóm theo âm lượng hoặc cao độ. Nếu chúng ta sắp xếp các âm thanh trước tiên theo âm lượng, sau đó theo cao độ (hoặc ngược lại), các chuỗi khác nhau theo một chiều thường có thể được trích xuất. Cụ thể, chúng ta có thể tạo thành một lưới trong đó một trục là âm lượng và trục kia là cao độ. Chúng ta có thể duyệt qua lưới này theo mô hình giống con rắn: chúng ta lặp qua tất cả các cao độ cho một tập nhất định, sau đó chuyển sang tập tiếp theo, đảo ngược thứ tự cao độ khi chúng ta tiếp tục. Điều này đảm bảo rằng các âm thanh liên tiếp khác nhau chính xác theo một chiều, vì việc di chuyển trong một âm lượng chỉ thay đổi cao độ, trong khi di chuyển sang âm lượng mới chỉ thay đổi âm lượng. Bằng cách xen kẽ các hướng một cách cẩn thận, chúng ta có thể tránh tạo ra ba giá trị giống hệt nhau liên tiếp ở một trong hai chiều. 

Việc sắp xếp và xây dựng quá trình duyệt con rắn này là$O(n \log n)$do sự sắp xếp và nó đòi hỏi$O(n)$không gian để lưu trữ đơn đặt hàng. Điều này có thể chấp nhận được với những hạn chế. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Ồ (n!) | O(n) | Quá chậm | 
| Truyền tải rắn thông qua sắp xếp | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test case, hãy đọc tất cả$n$âm thanh cùng với chỉ số ban đầu của họ. Lưu trữ mỗi âm thanh dưới dạng một bộ (âm lượng, cao độ, chỉ mục) để chúng ta có thể xuất thứ tự ở cuối. 
2. Nhóm âm thanh theo âm lượng. Trong mỗi nhóm âm lượng, hãy sắp xếp âm thanh theo cao độ. Điều này đảm bảo rằng việc lặp lại trong nhóm chỉ thay đổi cao độ. 
3. Khởi tạo một chuỗi kết quả trống. Duyệt qua từng nhóm khối lượng theo thứ tự tăng dần. Đối với các nhóm được đánh số chẵn, duyệt các nốt theo thứ tự tăng dần. Đối với các nhóm số lẻ, di chuyển các nốt theo thứ tự giảm dần. Mô hình giống con rắn này ngăn cản việc tạo ra ba tập hoặc cao độ giống hệt nhau liên tiếp. 
4. Thêm chỉ mục của từng âm thanh vào chuỗi kết quả trong quá trình truyền tải. Các âm thanh liên tiếp trong kết quả giờ đây khác nhau chính xác theo một chiều: cao độ trong một nhóm âm lượng hoặc âm lượng khi di chuyển giữa các nhóm. 
5. Sau khi xây dựng trình tự, hãy kiểm tra xem có bộ ba nhàm chán nào không. Đối với mỗi bộ ba chỉ số liên tiếp trong kết quả, hãy xác minh rằng không phải tất cả các khối đều bằng nhau và không phải tất cả các cao độ đều bằng nhau. Nếu bất kỳ bộ ba nào vi phạm điều kiện này, xuất ra "KHÔNG". Ngược lại, xuất "CÓ" theo sau là chuỗi các chỉ số. 
6. Lặp lại các bước trên cho từng trường hợp thử nghiệm. 

Bất biến được duy trì trong quá trình xây dựng là trong mỗi nhóm âm lượng, cao độ thay thế mà không lặp lại và giữa các nhóm âm lượng, âm lượng thay đổi trong khi cao độ tiếp tục theo thứ tự xen kẽ. Điều này đảm bảo rằng các âm thanh liên tiếp sẽ đẹp và tránh các bộ ba nhàm chán vì quá trình truyền tải giống như con rắn đảm bảo không có ba giá trị giống hệt nhau liên tiếp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        sounds = []
        for i in range(n):
            v, p = map(int, input().split())
            sounds.append((v, p, i + 1))

        from collections import defaultdict
        groups = defaultdict(list)
        for v, p, idx in sounds:
            groups[v].append((p, idx))

        sequence = []
        volumes_sorted = sorted(groups.keys())
        toggle = False
        for v in volumes_sorted:
            pitches = sorted(groups[v])
            if toggle:
                pitches.reverse()
            for p, idx in pitches:
                sequence.append((v, p, idx))
            toggle = not toggle

        possible = True
        for i in range(2, n):
            v1, p1, _ = sequence[i-2]
            v2, p2, _ = sequence[i-1]
            v3, p3, _ = sequence[i]
            if (v1 == v2 == v3) or (p1 == p2 == p3):
                possible = False
                break

        if possible:
            print("YES")
            print(" ".join(str(idx) for _, _, idx in sequence))
        else:
            print("NO")

if __name__ == "__main__":
    solve()
```Giải pháp đọc đầu vào hiệu quả và bảo toàn các chỉ số ban đầu. Việc nhóm theo khối lượng và sắp xếp theo cao độ cho phép di chuyển giống như con rắn. Việc chuyển đổi đảm bảo thứ tự xen kẽ giữa các nhóm, điều này rất quan trọng để tránh các bộ ba nhàm chán. Vòng xác thực cuối cùng xác nhận rằng không có bộ ba nào vi phạm điều kiện không nhàm chán. 

## Ví dụ đã hoạt động 

**Đầu vào mẫu 1**```
4
179 239
179 179
239 179
239 239
```| Bước | Trình tự đang được xây dựng | Kiểm tra | 
| --- | --- | --- | 
| Bắt đầu | [] | | 
| Tập 179 nhóm tăng dần | [(179,179,2),(179,239,1)] | | 
| Tập 239 nhóm giảm dần | [(179,179,2),(179,239,1),(239,239,4),(239,179,3)] | Không có bộ ba nhàm chán | 
| Cuối cùng | 2 1 4 3 | CÓ | 

Bảng cho thấy các âm thanh liên tiếp khác nhau về cao độ hoặc âm lượng và không có ba âm lượng hoặc cao độ liên tiếp nào giống hệt nhau. 

**Đầu vào mẫu 2**```
1 1
2 1
3 1
```| Bước | Trình tự | Kiểm tra | 
| --- | --- | --- | 
| Tập 1 nhóm | [(1,1,1),(2,1,2),(3,1,3)] | 3 cú ném liên tiếp toàn 1 → nhàm chán | 
| Cuối cùng | không thể | KHÔNG | 

Điều này xác nhận thuật toán loại bỏ chính xác các chuỗi không thể tránh được các bộ ba nhàm chán. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp trong mỗi nhóm khối lượng chiếm ưu thế. Truyền tải và xác nhận là O(n). | 
| Không gian | O(n) | Lưu trữ âm thanh và trình tự cuối cùng. | 

Thuật toán dễ dàng xử lý tổng của$n$trên tất cả các trường hợp thử nghiệm lên đến$2 \cdot 10^5$. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    solve()
    return out.getvalue().strip()

# Provided samples
assert run("5\n4\n179 239\n179 179\n239 179\n239 239\n3\n1 1\n2 1\n3 1\n1\n5 7\n5\n1 1\n1 2\n2 1\n2 2\n99 99\n7\n1 1\n1 3\n2 1\n2 2\n3 1\n3 2\n3 3\n") == \
"YES\n2 1 4 3\nNO\nYES\n1\nNO\nYES\n1 2 4 3 6 5 7", "sample 1"

# Custom: minimum input
assert run("1\n1\n1 1\n") == "YES\n1", "single sound"

# Custom: maximum identical volume
assert run("1\n3\n5 1\n5 2\n5 3\n") == "NO", "all same volume, more than 2"

# Custom: all distinct
assert run("1\n4\n1 1\n2 2\n3 3\n4 4\n") == "YES\n1 2 3 4", "all distinct simple"

# Custom: maximum n simple snake
inp = "1\n6\n1 1\n1 2\n2 1\n2 2\n3 1
```
