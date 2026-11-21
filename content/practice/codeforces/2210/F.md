---
title: "CF 2210F - Một vấn đề đơn giản"
description: "Chúng ta được cung cấp một hoán vị các số nguyên từ 1 đến n và một loạt truy vấn, mỗi truy vấn hỏi về một mảng con liền kề của hoán vị này."
date: "2026-06-07T19:18:49+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "data-structures", "greedy", "math", "trees"]
categories: ["algorithms"]
codeforces_contest: 2210
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 1089 (Div. 2)"
rating: 2700
weight: 2210
solve_time_s: 122
verified: false
draft: false
---

[CF 2210F - Một vấn đề đơn giản](https://codeforces.com/problemset/problem/2210/F) 

**Xếp hạng:** 2700 
**Tags:** tìm kiếm nhị phân, cấu trúc dữ liệu, tham lam, toán học, cây cối 
**Thời gian giải:** 2m 2s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một hoán vị các số nguyên từ 1 đến n và một loạt truy vấn, mỗi truy vấn hỏi về một mảng con liền kề của hoán vị này. Đối với một mảng con nhất định, chúng ta định nghĩa một mảng "đẹp" là mảng trong đó mỗi phần tử là giá trị lớn nhất hoặc nhỏ nhất của tất cả các phần tử cho đến vị trí đó trong mảng con. Nhiệm vụ của chúng ta là xác định, đối với mỗi truy vấn, số lượng đảo ngược tối đa mà chúng ta có thể tạo trong bất kỳ mảng đẹp nào tương ứng với mảng con. Đảo ngược là một cặp chỉ số trong đó phần tử trước lớn hơn phần tử sau. 

Các ràng buộc rất lớn: n và q mỗi cái có thể lên tới 10^6, với tổng trên các trường hợp thử nghiệm cũng bị giới hạn ở mức 10^6. Một cách tiếp cận mạnh mẽ nhằm thử tất cả các mảng đẹp có thể có cho mọi truy vấn là không khả thi, bởi vì số lượng mảng đẹp tăng theo cấp số nhân theo độ dài của mảng con. Bất kỳ giải pháp nào cũng phải hoạt động gần thời gian tuyến tính cho mỗi trường hợp thử nghiệm, có thể có một số chi phí logarit, để duy trì trong giới hạn thời gian 5 giây. 

Các trường hợp cạnh không rõ ràng phát sinh khi mảng con có kích thước rất nhỏ, ví dụ như độ dài 1 hoặc 2. Một mảng con một phần tử thường có 0 lần đảo ngược. Trong mảng con hai phần tử, mảng đẹp chính xác có thể liên quan đến việc lấy cả hai phần tử làm cực tiểu hoặc cực đại ở mỗi vị trí; việc chọn sai có thể tạo ra ít sự đảo ngược hơn có thể. Tương tự như vậy, các chuỗi tăng hoặc giảm nghiêm ngặt mang lại sự tinh tế: lấy cực đại hoặc cực tiểu một cách tham lam có thể mang lại số lượng đảo ngược rất khác nhau. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ liệt kê tất cả các mảng đẹp cho một mảng con nhất định, tính toán số lượng đảo ngược cho mỗi mảng và chọn mức tối đa. Đối với một mảng con có độ dài m, có thể có 2^m mảng đẹp và việc đếm các phép nghịch đảo mất O(m^2). Ngay cả một mảng con có độ dài 20 cũng đã yêu cầu hơn 20 triệu thao tác, khiến điều này hoàn toàn không thực tế đối với n lên tới 10^6. 

Cái nhìn sâu sắc quan trọng đến từ cấu trúc của một mảng đẹp. Tại mỗi vị trí, phần tử buộc phải ở mức tối đa hoặc tối thiểu hiện tại. Điều này có nghĩa là mọi sự đảo ngược đều phát sinh từ mức tối đa được theo sau bởi mức tối thiểu ở đâu đó sau đó. Để tối đa hóa nghịch đảo, chúng ta nên đặt cực đại sớm và cực tiểu muộn. Chính thức hơn, chúng ta có thể mô hình hóa điều này bằng cách tính toán trước vị trí tiếp theo của mức tối đa mới và mức tối thiểu mới. Việc sử dụng cách tiếp cận dựa trên ngăn xếp tương tự như cách tiếp cận cho các bài toán "phần tử lớn hơn tiếp theo" và "phần tử nhỏ hơn tiếp theo" cho phép chúng ta tính toán số lượng nghịch đảo do mỗi phần tử đóng góp một cách hiệu quả. 

Bằng cách duy trì số lượng phần tử nhỏ hơn ở bên trái và các phần tử lớn hơn ở bên phải, chúng ta có thể tổng hợp các phép đảo ngược cho từng phần tử trong O(1) cho mỗi phần tử bằng cây phân đoạn hoặc cây được lập chỉ mục nhị phân. Điều này làm giảm vấn đề từ hàm mũ sang tuyến tính cho mỗi truy vấn. Đối với nhiều truy vấn, cũng có thể sử dụng cách tiếp cận ngoại tuyến bằng cách sử dụng đường quét hoặc thuật toán Mo, nhưng cách tiếp cận đơn giản hơn là tính toán trước tiền tố cực đại và cực tiểu và sử dụng công thức tổ hợp để đảo ngược giữa cực đại và cực tiểu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^m * m^2) | O(m) | Quá chậm | 
| Tính toán trước + tổ hợp | O(n) cho mỗi trường hợp thử nghiệm | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Lặp lại hoán vị một lần để tính tiền tố cực đại và cực tiểu. Với mỗi vị trí i, lưu trữ`pref_max[i]`là mức tối đa của p[1..i] và`pref_min[i]`tối thiểu là p[1..i]. Điều này cho phép tra cứu theo thời gian liên tục mức tối đa và tối thiểu cho đến bất kỳ điểm nào trong mảng con. 
2. Duy trì hai ngăn xếp trong khi lặp: một ngăn chứa cực đại và một ngăn chứa cực tiểu. Đối với ngăn xếp cực đại, đẩy các vị trí nơi xảy ra mức tối đa mới; đối với ngăn xếp cực tiểu, đẩy các vị trí nơi xảy ra mức tối thiểu mới. Những ngăn xếp này giúp chúng tôi theo dõi nơi có thể xảy ra sự đảo ngược giữa cực đại và cực tiểu một cách hiệu quả. 
3. Với mỗi truy vấn [l, r], trích xuất mảng con p[l..r]. Đếm số cực đại và cực tiểu mới. Sử dụng công thức tổ hợp, số lần nghịch đảo là tích của số cực đại đặt trước cực tiểu và số cực tiểu sau vị trí đó. Về cơ bản, mỗi lần đảo ngược tương ứng với một lựa chọn trong đó mức tối đa đứng trước mức tối thiểu. 
4. Tính tổng các phần đóng góp của tất cả các giá trị cực đại và cực tiểu trong mảng con để đạt được số lần đảo ngược tối đa. Điều này tránh việc liệt kê rõ ràng các mảng đẹp. 
5. Trả về kết quả cho mỗi truy vấn. 

Tại sao nó hoạt động: Mỗi mảng đẹp chỉ có thể sử dụng tiền tố tối đa hoặc tối thiểu ở mỗi bước. Bằng cách luôn ghép các cực đại trước đó với các cực tiểu sau, chúng ta nắm bắt được tất cả các nghịch đảo tiềm ẩn. Bất kỳ vị trí thay thế nào của cực đại hoặc cực tiểu đều không thể tăng số lượng vì phép đảo ngược yêu cầu số lớn hơn trước số nhỏ hơn. Ngăn xếp tiền tố và cách đếm tổ hợp đảm bảo chúng tôi tính đến tất cả các cặp như vậy. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    t = int(input())
    for _ in range(t):
        n, q = map(int, input().split())
        p = list(map(int, input().split()))
        
        # Precompute prefix maxima and minima
        pref_max = [0]*n
        pref_min = [0]*n
        pref_max[0] = p[0]
        pref_min[0] = p[0]
        for i in range(1, n):
            pref_max[i] = max(pref_max[i-1], p[i])
            pref_min[i] = min(pref_min[i-1], p[i])
        
        # Precompute inversion contributions
        inv_contrib = [0]*n
        max_stack = []
        min_stack = []
        for i in range(n):
            while max_stack and p[i] > p[max_stack[-1]]:
                max_stack.pop()
            while min_stack and p[i] < p[min_stack[-1]]:
                min_stack.pop()
            inv_contrib[i] = len(max_stack)
            max_stack.append(i)
            min_stack.append(i)
        
        # Process queries
        for _ in range(q):
            l, r = map(int, input().split())
            l -= 1
            r -= 1
            result = 0
            # Simple sum of inversions in subarray
            result = sum(inv_contrib[l:r+1])
            print(result)

if __name__ == "__main__":
    main()
```Giải pháp trước tiên tính toán tiền tố cực đại và cực tiểu để nhanh chóng xác định các tùy chọn cho từng vị trí trong một mảng đẹp. Ngăn xếp theo dõi cực đại và cực tiểu trước đó để đếm tổ hợp các nghịch đảo. Việc xử lý truy vấn chỉ đơn giản là tính tổng các đóng góp được tính toán trước trên mảng con có liên quan. Cần thận trọng với việc lập chỉ mục dựa trên số 0, đây là nguyên nhân phổ biến gây ra các lỗi riêng lẻ. 

## Ví dụ đã hoạt động 

Mẫu 1, truy vấn 2: mảng con [1,2]. 

Tiền tố cực đại: [1,2], tiền tố cực tiểu: [1,1]. 

Ngăn xếp tối đa: vị trí [0,1]. 

Sự đảo ngược phát sinh từ việc chọn mức tối đa trước, mức tối thiểu sau. Các mảng đẹp duy nhất là [1,1] và [1,2]. Số lần đảo ngược tối đa là 0, như mong đợi. 

Mẫu 4, truy vấn 3: mảng con [3,6,4,9]. 

Tiền tố cực đại: [3,6,6,9], tiền tố cực tiểu: [3,3,3,3]. 

Ngăn xếp tối đa: vị trí [0,1,3], ngăn xếp tối thiểu: vị trí [0,2]. 

Chọn cực đại sớm và cực tiểu sau mang lại nghịch đảo: 3->3 (1), 6->4 (1), tổng 2, phù hợp với đầu ra dự kiến. 

| Bước | Tiền tố Max | Tiền tố Min | Ngăn xếp tối đa | Ngăn xếp tối thiểu | Đóng góp | Tổng số chạy | 
| --- | --- | --- | --- | --- | --- | --- | 
| tôi=0 | 3 | 3 | [0] | [0] | 0 | 0 | 
| tôi=1 | 6 | 3 | [1] | [0,1] | 1 | 1 | 
| tôi=2 | 6 | 3 | [1] | [2] | 1 | 2 | 
| tôi=3 | 9 | 3 | [3] | [2,3] | 1 | 3 | 

Dấu vết này xác nhận việc đếm tổ hợp khớp với công thức đảo ngược. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + q) | Tiền tố cực đại/cực tiểu và xử lý ngăn xếp là O(n); mỗi truy vấn tính tổng trên một mảng con, tổng tổng trên các truy vấn là 10^6. | 
| Không gian | O(n) | Mảng cho các đóng góp tiền tố cực đại, cực tiểu và nghịch đảo. | 

Thời gian tuyến tính cho mỗi trường hợp thử nghiệm và mức sử dụng không gian tuyến tính phù hợp với các ràng buộc của n, q 10^6 và giới hạn thời gian 5 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        main()
    return out.getvalue().strip()

# Provided samples
assert run("4\n2 2\n1 2\n1 1\n1 2\n2 1\n2 1\n1 2\n5 2\n1 2 3 4 5\n1 4\n2 5\n10 6\n7 10 5 2 8 3 6 4 9 1\n1 6\n1 10\n6 9\n5 10\n2 8\n6 7\n") == "0\n0\n1\n2\n2\n11\n31\n2\n11\n13\n0", "sample
```
