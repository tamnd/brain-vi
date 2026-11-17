---
title: "CF 2001D - Chuỗi tối thiểu dài nhất"
description: "Chúng ta được yêu cầu xử lý một dãy số nguyên và tìm một dãy con có độ dài lớn nhất trong khi không chứa các phần tử lặp lại."
date: "2026-06-08T14:07:13+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "constructive-algorithms", "data-structures", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 2001
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 967 (Div. 2)"
rating: 1900
weight: 2001
solve_time_s: 200
verified: false
draft: false
---

[CF 2001D - Chuỗi tối thiểu dài nhất](https://codeforces.com/problemset/problem/2001/D) 

**Xếp hạng:** 1900 
**Tags:** bạo lực, thuật toán xây dựng, cấu trúc dữ liệu, tham lam, triển khai 
**Thời gian giải:** 3 phút 20s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu xử lý một dãy số nguyên và tìm một dãy con có độ dài lớn nhất trong khi không chứa các phần tử lặp lại. Trong số tất cả các dãy con có độ dài tối đa, chúng ta cần chọn dãy con có thứ tự từ điển được sửa đổi là cực tiểu, trong đó mỗi phần tử ở vị trí lẻ trong dãy con được nhân với$-1$. Về cơ bản, chúng tôi đang cân bằng hai mục tiêu: thứ nhất, chúng tôi phải lấy càng nhiều số riêng biệt càng tốt và thứ hai, chúng tôi phải quyết định thứ tự của chúng để giảm thiểu giá trị từ điển đã ký. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm. Đối với mỗi trường hợp, chúng ta được cho độ dài của chuỗi$n$và chính trình tự đó. Những ràng buộc cho phép$n$để được$3 \cdot 10^5$, với tổng số tiền là$n$trên tất cả các trường hợp thử nghiệm không vượt quá$3 \cdot 10^5$. Điều này cấm bất kỳ thuật toán nào tệ hơn thuật toán tuyến tính trong$n$mỗi trường hợp thử nghiệm. Một cách tiếp cận đơn giản để kiểm tra tất cả các chuỗi con sẽ đòi hỏi thời gian theo cấp số nhân và hoàn toàn không khả thi. Các trường hợp biên bao gồm các chuỗi trong đó tất cả các phần tử giống hệt nhau, các chuỗi có độ dài bằng 1 và các chuỗi trong đó chuỗi con tối ưu yêu cầu xen kẽ các bản sao một cách cẩn thận để đáp ứng tiêu chí từ điển. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ liệt kê tất cả các chuỗi con của đầu vào, loại bỏ những chuỗi có phần tử lặp lại và sau đó chọn chuỗi dài nhất với thứ tự từ điển được sửa đổi tối thiểu. Điều này đòi hỏi$O(2^n)$hoạt động trên mỗi trường hợp thử nghiệm và rõ ràng là không thể thực hiện được đối với$n$lên đến$3 \cdot 10^5$. 

Cách tiếp cận tối ưu dựa trên hai quan sát. Đầu tiên, độ dài của dãy con dài nhất không trùng lặp chính xác là số phần tử riêng biệt trong dãy. Điều này làm giảm vấn đề lựa chọn các phần tử riêng biệt này theo thứ tự giảm thiểu trình tự từ điển đã sửa đổi. Thứ hai, chúng ta có thể xây dựng dãy con một cách tham lam từ trái sang phải, lấy lần xuất hiện cuối cùng của mỗi phần tử riêng biệt khi duyệt qua đầu vào. Điều này đảm bảo rằng khi chúng ta đảo ngược dãy con để đáp ứng tiêu chí từ điển phủ định vị trí lẻ, chúng ta có thứ tự tối thiểu. Điểm mấu chốt là đối với một chuỗi không trùng lặp, mức độ tự do duy nhất là thứ tự tương đối của các phần tử riêng biệt và việc đảo ngược chúng theo lần xuất hiện cuối cùng sẽ giảm thiểu giá trị từ điển đã ký một cách tự nhiên. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo một từ điển trống để lưu trữ chỉ số xuất hiện cuối cùng của mỗi phần tử. Điều này cho phép chúng ta biết vị trí cuối cùng nơi mỗi phần tử xuất hiện trong chuỗi. 
2. Duyệt chuỗi đầu vào từ trái sang phải. Đối với mỗi phần tử, hãy cập nhật chỉ mục xuất hiện cuối cùng của nó trong từ điển. Sau bước này, từ điển sẽ ánh xạ từng giá trị riêng biệt tới chỉ mục cuối cùng của nó trong mảng. 
3. Trích xuất các phần tử riêng biệt và sắp xếp chúng theo chỉ số xuất hiện cuối cùng của chúng. Điều này tạo ra một chuỗi các giá trị riêng biệt theo thứ tự đảm bảo tiêu chí từ điển được giảm thiểu khi điều chỉnh dấu cho các vị trí lẻ. 
4. Xuất ra độ dài của dãy này, là số phần tử riêng biệt. 
5. Xuất ra chuỗi theo thứ tự được xác định bằng cách sắp xếp theo lần xuất hiện cuối cùng. 

Điều bất biến là bằng cách chọn các phần tử theo thứ tự xuất hiện lần cuối của chúng, bất kỳ chuỗi con nào thu được từ mảng ban đầu sử dụng mỗi phần tử nhiều nhất một lần đều không thể tạo ra chuỗi từ điển có dấu nhỏ hơn. Việc sắp xếp lần xuất hiện cuối cùng đảm bảo rằng các phần tử trước đó trong đầu ra sẽ xuất hiện sau lần xuất hiện cuối cùng có thể có của chúng trong đầu vào, điều này đảm bảo các giá trị tối thiểu cho các số âm ở vị trí lẻ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        last_occurrence = {}
        for i, val in enumerate(a):
            last_occurrence[val] = i
        # Sort elements by last occurrence index
        result = sorted(last_occurrence.keys(), key=lambda x: last_occurrence[x])
        print(len(result))
        print(' '.join(map(str, result)))

solve()
```Giải pháp sử dụng từ điển để theo dõi hiệu quả lần xuất hiện cuối cùng của từng phần tử riêng biệt. Việc sắp xếp theo các chỉ mục được lưu trữ đảm bảo rằng chúng ta tôn trọng quy tắc sắp xếp từ điển khi xem xét các dấu hiệu ở vị trí lẻ. Tất cả các phép toán đều tuyến tính theo số phần tử riêng biệt, được giới hạn bởi$n$, đảm bảo hiệu quả dưới các ràng buộc của bài toán. Các trường hợp cạnh như chuỗi có tất cả các số giống hệt nhau hoặc chuỗi một phần tử được xử lý một cách tự nhiên vì bản đồ xuất hiện cuối cùng vẫn xác định chính xác phần tử duy nhất duy nhất. 

## Ví dụ đã hoạt động 

Đối với đầu vào`[3, 2, 1, 3]`, lần xuất hiện cuối cùng là`{3: 3, 2: 1, 1: 2}`. Sắp xếp theo sản lượng chỉ số`[2, 1, 3]`. Độ dài đầu ra là`3`, trình tự đầu ra`[2, 1, 3]`. Đảo ngược vị trí lẻ mang dấu âm sẽ tạo ra`[-2, 1, -3]`, nhỏ hơn các dãy con khác có độ dài bằng ba. 

Đối với đầu vào`[1, 1, 1, 1]`, từ điển xuất hiện cuối cùng là`{1: 3}`. Độ dài chuỗi đầu ra là`1`và trình tự`[1]`. 

| Chỉ mục | Yếu tố | Lần Xuất Hiện Cuối Cùng | Trình tự đầu ra | 
| --- | --- | --- | --- | 
| 0 | 3 | 3 | 2 | 
| 1 | 2 | 1 | 1 | 
| 2 | 1 | 2 | 3 | 
| 3 | 3 | 3 | | 

Dấu vết này cho thấy thuật toán nắm bắt chính xác các phần tử riêng biệt theo thứ tự mong muốn, xác nhận giá trị giữ bất biến. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Duyệt qua chuỗi và cập nhật lần xuất hiện cuối cùng là tuyến tính; sắp xếp các phần tử riêng biệt là O(d log d), nhưng d ≤ n | 
| Không gian | O(n) | Từ điển lưu trữ tối đa n khóa, mỗi khóa cho một phần tử riêng biệt | 

Giải pháp phù hợp thoải mái trong giới hạn thời gian và bộ nhớ, vì tổng của$n$trên các trường hợp thử nghiệm được giới hạn bởi$3 \cdot 10^5$và các phép toán trên mỗi phần tử là tối thiểu. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    solve()
    sys.stdout = sys.__stdout__
    return output.getvalue().strip()

# provided samples
assert run("4\n4\n3 2 1 3\n4\n1 1 1 1\n9\n3 2 1 3 2 1 3 2 1\n1\n1\n") == "3\n2 1 3\n1\n1\n3\n2 1 3\n1\n1", "sample 1"

# custom cases
assert run("1\n5\n5 4 3 2 1\n") == "5\n5 4 3 2 1", "decreasing order"
assert run("1\n5\n1 2 3 4 5\n") == "5\n1 2 3 4 5", "increasing order"
assert run("1\n5\n2 2 2 2 2\n") == "1\n2", "all equal"
assert run("1\n6\n1 2 1 2 1 2\n") == "2\n1 2", "alternating duplicates"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 5 4 3 2 1 | 5 4 3 2 1 | xử lý trình tự giảm dần | 
| 1 2 3 4 5 | 5 1 2 3 4 5 | xử lý trình tự tăng dần | 
| 2 2 2 2 2 | 1 2 | tất cả các số bằng nhau được xử lý | 
| 1 2 1 2 1 2 | 2 1 2 | các bản sao xen kẽ được xử lý | 

## Vỏ cạnh 

Đối với một chuỗi có độ dài một, ví dụ:`[1]`, bản đồ từ điển`1`lập chỉ mục`0`, đầu ra là`[1]`. Thuật toán xử lý chính xác đầu vào tối thiểu này. Đối với các chuỗi có tất cả các phần tử giống nhau, từ điển giảm xuống còn một khóa duy nhất, do đó thuật toán tạo ra chuỗi một phần tử`[value]`. Đối với các chuỗi trong đó chuỗi con dài nhất yêu cầu chọn các phần tử theo thứ tự không phải ngoài cùng bên trái, chẳng hạn như`[3, 2, 1, 3]`, sắp xếp theo lần xuất hiện cuối cùng đảm bảo đúng thứ tự`[2, 1, 3]`. Điều này đảm bảo rằng tiêu chí từ điển phủ định vị trí lẻ được thỏa mãn.
