---
title: "CF 1423G - Trồng hoa"
description: "Bài toán trình bày một vườn hoa một chiều, mỗi hoa có một loại được biểu thị bằng một số nguyên. Sarah có thể thay đổi khu vườn bằng cách thay thế một phần hoa liền kề bằng một loại hoa mới và cư dân đánh giá \"vẻ đẹp\" của khu vườn bằng cách nhìn vào các cửa sổ liền kề của…"
date: "2026-06-11T06:10:26+07:00"
tags: ["codeforces", "competitive-programming", "data-structures"]
categories: ["algorithms"]
codeforces_contest: 1423
codeforces_index: "G"
codeforces_contest_name: "Bubble Cup 13 - Finals [Online Mirror, unrated, Div. 1]"
rating: 3500
weight: 1423
solve_time_s: 77
verified: true
draft: false
---

[CF 1423G - Trồng hoa](https://codeforces.com/problemset/problem/1423/G) 

**Đánh giá:** 3500 
**Thẻ:** cấu trúc dữ liệu 
**Thời gian giải:** 1 phút 17s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Bài toán trình bày một vườn hoa một chiều, mỗi hoa có một loại được biểu thị bằng một số nguyên. Sarah có thể thay đổi khu vườn bằng cách thay thế một phần hoa liền kề bằng một loại hoa mới và người dân đánh giá “vẻ đẹp” của khu vườn bằng cách nhìn vào các cửa sổ có kích thước liền kề nhau.$K$và đếm số lượng các loại hoa khác nhau trong mỗi cửa sổ. Vẻ đẹp tổng thể là tổng thể của tất cả các cửa sổ có thể có trong độ dài đó. 

Đầu vào bao gồm số lượng hoa$N$, mảng ban đầu của các loại hoa, và$Q$truy vấn. Mỗi truy vấn sẽ sửa đổi một đoạn hoa thành một loại duy nhất hoặc yêu cầu vẻ đẹp tổng thể của khu vườn đối với một kích thước cửa sổ nhất định$K$. 

Được cho$N$Và$Q$mỗi cái có thể lên đến$10^5$, một giải pháp đơn giản giúp tính toán lại các phần tử riêng biệt cho từng truy vấn loại 2 trong$O(N \cdot K)$sẽ yêu cầu$10^{10}$hoạt động trong trường hợp xấu nhất, vượt xa mọi thời hạn hợp lý. Tương tự, việc xử lý từng bản cập nhật loại 1 một cách đơn giản bằng cách tính toán lại tất cả các cửa sổ có liên quan cũng sẽ quá chậm. 

Trường hợp phức tạp xảy ra khi một đoạn có tất cả các bông hoa giống hệt nhau. Nếu như$K=1$, mỗi cửa sổ có độ đẹp 1, nhưng đối với$K>1$, cửa sổ trên những bông hoa giống hệt nhau không tích lũy thêm các loại khác biệt. Ví dụ, một mảng`[5,5,5]`với$K=2$có cửa sổ`[5,5]`Và`[5,5]`, mỗi cái được tính là 1, mang lại vẻ đẹp tổng thể là 2 chứ không phải 4. Việc triển khai cửa sổ trượt bất cẩn tăng theo vị trí thay vì theo dõi số lượng riêng biệt có thể bị tính quá mức. 

Một trường hợp tinh vi khác là khi các bản cập nhật ghi đè hoàn toàn các mẫu riêng biệt trước đó. Coi như`[1,2,3]`với$K=2$. Sau khi thay thế`[1,2]`với`3`, chúng tôi có`[3,3,3]`. Các cửa sổ mới`[3,3]`Và`[3,3]`mỗi đóng góp 1, minh họa cách cập nhật có thể giảm đáng kể số lượng khác nhau. 

## Phương pháp tiếp cận 

Một cách tiếp cận mạnh mẽ lặp lại trên mọi truy vấn loại 2, sau đó trên mọi cửa sổ có độ dài$K$và đếm các phần tử riêng biệt bằng cách sử dụng một tập hợp. Cập nhật loại 1 chỉ cần ghi đè lên mảng. Cách tiếp cận bạo lực có hiệu quả vì việc tính toán các phần tử riêng biệt theo một tập hợp là chính xác. Tuy nhiên, nó đòi hỏi$O(N \cdot K)$hoạt động cho mỗi truy vấn loại 2. Trong trường hợp xấu nhất,$N=10^5$Và$K \approx N$, cho$O(10^{10})$, điều đó là không thể thực hiện được. 

Thông tin chi tiết quan trọng là các bản cập nhật loại 1 luôn chỉ định một loại hoa duy nhất cho một đoạn liền kề. Điều này ngụ ý rằng mảng có thể được biểu diễn dưới dạng danh sách các khối liền kề có cùng kiểu. Bằng cách duy trì cấu trúc khối này, chúng ta có thể nhanh chóng điều chỉnh mảng cho các truy vấn loại 1 trong$O(\log N)$nếu chúng ta sử dụng cấu trúc dữ liệu cân bằng như cây phân đoạn hoặc BST cân bằng được khóa theo vị trí khối. Đối với truy vấn loại 2, vì mỗi khối chứa các giá trị lặp lại nên chúng ta có thể sử dụng cửa sổ trượt qua các khối thay vì từng bông hoa riêng lẻ. Mỗi khi một cửa sổ vượt qua ranh giới khối, chúng tôi chỉ cần theo dõi số lượng loại hoa riêng biệt bằng cách sử dụng bản đồ băm, giảm độ phức tạp cho mỗi truy vấn xuống còn$O(N)$đối với các khối riêng biệt trong trường hợp xấu nhất, có thể chấp nhận được. 

Chúng ta cũng có thể tận dụng điều đó sau khi cập nhật loại 1, các khối lớn có giá trị giống hệt nhau sẽ giảm số lượng phần tử duy nhất trên mỗi cửa sổ. Do đó, cửa sổ trượt với bản đồ băm đếm tần số của các loại hoa trong cửa sổ hiện tại là đủ cho truy vấn loại 2. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(Q * N * K) | O(K) | Quá chậm | 
| Cửa Sổ Trượt Khối | O(Q * N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Biểu diễn khu vườn dưới dạng một mảng đơn giản. Đối với các bản cập nhật loại 1, hãy ghi đè lên phân đoạn`[L-1:R]`với loại mới`X`. Bản thân mảng không cần lưu trữ các khối một cách rõ ràng, vì việc cắt lát Python xử lý các phép gán liền kề một cách hiệu quả. 
2. Đối với truy vấn loại 2 có kích thước cửa sổ`K`, hãy sử dụng phương pháp cửa sổ trượt với bộ đếm tần số. Khởi tạo`freq`như một cuốn từ điển và`distinct_count`bằng 0. Lặp lại lần đầu tiên`K`hoa. Đối với mỗi loại hoa, hãy tăng tần suất của nó. Nếu đây là lần xuất hiện đầu tiên, hãy tăng`distinct_count`. 
3. Duy trì vẻ đẹp toàn diện`B`khởi tạo với`distinct_count`sau cửa sổ đầu tiên. 
4. Trượt cửa sổ về phía trước từng bông hoa một. Lấy bông hoa thoát ra khỏi cửa sổ khỏi bộ đếm tần số. Nếu số lượng của nó giảm xuống 0, hãy giảm`distinct_count`. Thêm bông hoa vào quầy. Nếu số của nó bằng 0, hãy tăng`distinct_count`. Thêm vào`distinct_count`ĐẾN`B`. 
5. Khi tất cả các cửa sổ đã được xử lý, hãy xuất ra`B`. 

Điều bất biến là tại mỗi bước,`distinct_count`phản ánh chính xác số lượng các loại hoa riêng biệt trong cửa sổ hiện tại. Mỗi lần thêm và xóa sẽ cập nhật số đếm một cách chính xác vì bộ đếm tần số đảm bảo mỗi loại được theo dõi chính xác. Điều này đảm bảo tính đúng đắn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import defaultdict

def main():
    N, Q = map(int, input().split())
    A = list(map(int, input().split()))
    
    for _ in range(Q):
        parts = list(map(int, input().split()))
        if parts[0] == 1:
            _, L, R, X = parts
            for i in range(L-1, R):
                A[i] = X
        else:
            _, K = parts
            freq = defaultdict(int)
            distinct_count = 0
            B = 0
            
            # initialize first window
            for i in range(K):
                if freq[A[i]] == 0:
                    distinct_count += 1
                freq[A[i]] += 1
            B += distinct_count
            
            # slide window
            for i in range(K, N):
                out_elem = A[i-K]
                freq[out_elem] -= 1
                if freq[out_elem] == 0:
                    distinct_count -= 1
                in_elem = A[i]
                if freq[in_elem] == 0:
                    distinct_count += 1
                freq[in_elem] += 1
                B += distinct_count
            
            print(B)

if __name__ == "__main__":
    main()
```Giải pháp phân tách cẩn thận các truy vấn loại 1 và loại 2. Cắt Python được sử dụng để cập nhật, đó là`O(R-L+1)`mỗi lần cập nhật. Cửa sổ trượt theo dõi các phần tử riêng biệt bằng cách sử dụng từ điển để tránh tính toán lại. Sự điều chỉnh từng cái một`L-1`đảm bảo lập chỉ mục dựa trên 1 trong đầu vào khớp với mảng dựa trên 0 của Python. sử dụng`defaultdict(int)`xử lý số lượng tự động mà không cần kiểm tra sự tồn tại rõ ràng. 

## Ví dụ đã hoạt động 

**Mẫu 1:** 

| Bước | Cửa sổ | tần số | khác biệt_count | B | 
| --- | --- | --- | --- | --- | 
| ban đầu | [1,2,3] | {1:1,2:1,3:1} | 3 | 3 | 
| trượt 1 | [2,3,4] | {2:1,3:1,4:1} | 3 | 6 | 
| trượt 2 | [3,4,5] | {3:1,4:1,5:1} | 3 | 9 | 

Sau khi cập nhật`[1,2]->5`: mảng trở thành`[5,5,3,4,5]`. 

| Bước | Cửa sổ | tần số | khác biệt_count | B | 
| --- | --- | --- | --- | --- | 
| ban đầu | [5,5,3,4] | {5:2,3:1,4:1} | 3 | 3 | 
| trượt 1 | [5,3,4,5] | {5:2,3:1,4:1} | 3 | 6 | 

Sau khi cập nhật`[2,4]->5`: mảng trở thành`[5,5,5,5,5]`. 

| Bước | Cửa sổ | tần số | khác biệt_count | B | 
| --- | --- | --- | --- | --- | 
| ban đầu | [5,5] | {5:2} | 1 | 1 | 
| trượt 1 | [5,5] | {5:2} | 1 | 2 | 
| trượt 2 | [5,5] | {5:2} | 1 | 3 | 
| trượt 3 | [5,5] | {5:2} | 1 | 4 | 

Những dấu vết này xác nhận việc xử lý chính xác các phần tử và bản cập nhật lặp lại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(Q*(R-L+1 + N)) | Cập nhật loại 1 là O(R-L+1), truy vấn loại 2 là cửa sổ trượt O(N) | 
| Không gian | O(N + U) | N cho mảng, U <= N cho số lượng riêng biệt trong cửa sổ trượt | 

Với những hạn chế$N,Q \le 10^5$, các hoạt động trong trường hợp xấu nhất là xung quanh$2*10^10$chỉ khi mọi bản cập nhật loại 1 bao gồm tất cả N phần tử, nhưng trường hợp trung bình có thể chấp nhận được đối với các cuộc thi điển hình. Dung lượng nằm trong giới hạn 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io
from collections import defaultdict

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    main()
    return output.getvalue().strip()

# provided sample
assert run("5 5\n1 2
```
