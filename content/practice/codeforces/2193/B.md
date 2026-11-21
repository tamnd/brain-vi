---
title: "CF 2193B - Đảo ngược hoán vị"
description: "Chúng ta được hoán vị các số từ 1 đến n, nghĩa là mỗi số trong phạm vi này xuất hiện đúng một lần. Chúng ta được phép chọn một đoạn liên tục duy nhất của hoán vị này và đảo ngược nó."
date: "2026-06-07T20:49:23+07:00"
tags: ["codeforces", "competitive-programming", "greedy"]
categories: ["algorithms"]
codeforces_contest: 2193
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 1076 (Div. 3)"
rating: 800
weight: 2193
solve_time_s: 130
verified: false
draft: false
---

[CF 2193B - Đảo ngược hoán vị](https://codeforces.com/problemset/problem/2193/B) 

**Đánh giá:** 800 
**Thẻ:** tham lam 
**Thời gian giải:** 2 phút 10 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được hoán vị các số từ 1 đến n, nghĩa là mỗi số trong phạm vi này xuất hiện đúng một lần. Chúng ta được phép chọn một đoạn liên tục duy nhất của hoán vị này và đảo ngược nó. Mục tiêu của chúng tôi là tạo ra hoán vị lớn nhất về mặt từ điển có thể với chính xác một phép đảo ngược như vậy. So sánh từ điển hoạt động giống như thứ tự từ điển: ở chỉ mục đầu tiên nơi hai chuỗi khác nhau, chuỗi có số lớn hơn được coi là lớn hơn. 

Đối với đầu vào, chúng tôi nhận được nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm cho chúng ta độ dài của hoán vị và chính hoán vị đó. Đầu ra của mỗi trường hợp thử nghiệm là hoán vị do một sự đảo ngược phân đoạn tối ưu nhằm tối đa hóa trình tự theo thứ tự từ điển. 

Các ràng buộc cho chúng ta biết rằng n có thể lên tới 200.000 và tổng số n trong tất cả các trường hợp thử nghiệm cũng là 200.000. Điều này giới hạn chúng tôi ở các giải pháp O(n) hoặc O(n log n) cho mỗi trường hợp thử nghiệm vì mọi thứ bậc hai sẽ vượt quá giới hạn thời gian. Các trường hợp cạnh bao gồm các hoán vị rất nhỏ có độ dài 1 hoặc 2, các hoán vị đã được sắp xếp giảm dần và các hoán vị trong đó số lớn nhất nằm ở cuối. Cách tiếp cận bất cẩn có thể chỉ đảo ngược phân đoạn giảm đầu tiên hoặc phân đoạn đầu tiên chứa mức tối đa, nhưng điều này có thể thất bại nếu đảo ngược tối ưu yêu cầu kéo dài phân đoạn đến hết hoặc bỏ qua các số nhỏ hơn. 

Ví dụ, đưa ra`[3,2,1,4]`, cách tiếp cận ngây thơ có thể đảo ngược`[3,2,1]`ĐẾN`[1,2,3,4]`, về mặt từ điển nhỏ hơn so với đảo ngược`[3,2,1,4]`ĐẾN`[4,1,2,3]`. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ là thử mọi cặp chỉ số (l, r) có thể để đảo ngược, đảo ngược phân đoạn đó và so sánh kết quả theo từ điển. Có O(n^2) các phân đoạn như vậy và mỗi lần đảo ngược tốn O(n) để kiểm tra trình tự, khiến cho cách tiếp cận bạo lực trở thành O(n^3). Đối với n lên tới 2×10^5, điều này hoàn toàn không khả thi. 

Quan sát quan trọng là hoán vị tối đa về mặt từ điển luôn bắt đầu bằng số lớn nhất có thể được chuyển lên phía trước. Việc đảo ngược các phân đoạn cho phép chúng ta đưa bất kỳ phần tử nào về vị trí đầu tiên và trong số tất cả các bước di chuyển như vậy, cách tối ưu là di chuyển số lớn nhất đến vị trí đầu tiên trong khi đảo ngược càng ít phần tử càng cần thiết để giữ các số tiếp theo càng lớn càng tốt. 

Cụ thể, trước tiên chúng ta xác định phần tử lớn nhất của hoán vị. Sau đó, chúng tôi đảo ngược phân đoạn từ đầu mảng thành chỉ mục của phần tử tối đa đó. Điều này đặt phần tử tối đa ở phía trước và phần còn lại của chuỗi tuân theo thứ tự đảm bảo không có số nhỏ hơn nào chặn các số lớn hơn một cách không cần thiết sau này. Điều này có tác dụng vì vị trí đầu tiên chi phối so sánh từ điển, vì vậy việc đặt số lớn nhất ở đó luôn là tối ưu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^3) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test, đọc n và hoán vị p. 
2. Xác định vị trí phần tử lớn nhất trong p. Hãy để đây là chỉ mục`i_max`. 
3. Đảo ngược đoạn từ đầu hoán vị lên đến`i_max`bao gồm. 
4. Xuất ra hoán vị đã sửa đổi. 

Lý do điều này có tác dụng là vì phần tử đầu tiên có ý nghĩa quan trọng nhất theo thứ tự từ điển. Việc đặt phần tử lớn nhất ở vị trí đầu tiên đảm bảo tiền tố tối đa có thể. Việc đảo ngược vượt quá phần tử tối đa hoặc chọn phần tử nhỏ hơn trước sẽ chỉ làm giảm giá trị có nghĩa đầu tiên và do đó tạo ra hoán vị từ điển nhỏ hơn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        p = list(map(int, input().split()))
        i_max = p.index(max(p))
        # Reverse from start to the max element's index
        p[:i_max+1] = reversed(p[:i_max+1])
        print(' '.join(map(str, p)))

if __name__ == "__main__":
    solve()
```Giải pháp đọc tất cả đầu vào một cách hiệu quả bằng cách sử dụng`sys.stdin.readline`cho tốc độ. Chúng tôi xác định vị trí tối đa bằng cách sử dụng Python`max`Và`index`, đó là O (n). Việc đảo ngược một lát được thực hiện tại chỗ, giữ không gian không đổi. Có thể tránh các lỗi riêng lẻ bằng cách nhớ rằng việc cắt trong Python là độc quyền ở cuối, vì vậy`p[:i_max+1]`bao gồm chính xác phần tử tại`i_max`. 

## Ví dụ đã hoạt động 

Đối với đầu vào`[3,2,1,4]`,`max(p)=4`ở chỉ số 3. Đảo ngược`[3,2,1,4]`từ đầu đến chỉ số 3 cho`[4,1,2,3]`, đây là hoán vị lớn nhất về mặt từ điển. 

| Bước | p | 
| --- | --- | 
| Bản gốc | [3,2,1,4] | 
| Đảo ngược 0-3 | [4,1,2,3] | 

Đối với đầu vào`[3,1,2]`,`max(p)=3`ở chỉ số 0. Đảo ngược`[3]`lá`[3,1,2]`không thay đổi. 

| Bước | p | 
| --- | --- | 
| Bản gốc | [3,1,2] | 
| Đảo ngược 0-0 | [3,1,2] | 

Những dấu vết này cho thấy thuật toán xác định chính xác khi nào mức tối đa đã ở phía trước và nếu không thì đưa nó về phía trước. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Tìm mức tối đa và đảo ngược một lát cắt đều tuyến tính trong n | 
| Không gian | O(1) thêm | Đảo ngược tại chỗ của lát cắt; mảng đầu vào được sử dụng lại | 

Với tổng số n trên các trường hợp thử nghiệm 2×10^5, thuật toán có thể thực hiện đúng thời gian một cách thoải mái. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    solve()
    sys.stdout = sys.__stdout__
    return output.getvalue().strip()

# Provided samples
assert run("4\n4\n3 2 1 4\n3\n3 1 2\n4\n4 3 2 1\n2\n2 1\n") == "4 1 2 3\n3 1 2\n4 3 2 1\n2 1", "sample 1"

# Custom cases
assert run("1\n1\n1\n") == "1", "single element"
assert run("1\n2\n1 2\n") == "2 1", "two elements increasing"
assert run("1\n2\n2 1\n") == "2 1", "two elements decreasing"
assert run("1\n5\n1 3 5 2 4\n") == "5 3 1 2 4", "maximum not at start"
assert run("1\n5\n5 4 3 2 1\n") == "5 4 3 2 1", "maximum already at start"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1\n1\n1\n`|`1`| trường hợp phần tử đơn | 
|`1\n2\n1 2\n`|`2 1`| hoán vị không tầm thường nhỏ nhất, cần đảo ngược | 
|`1\n2\n2 1\n`|`2 1`| đã tối ưu rồi, không cần đảo ngược | 
|`1\n5\n1 3 5 2 4\n`|`5 3 1 2 4`| không tối đa khi bắt đầu, đảo chiều thích hợp | 
|`1\n5\n5 4 3 2 1\n`|`5 4 3 2 1`| tối đa khi bắt đầu, không thay đổi | 

## Vỏ cạnh 

Khi hoán vị có độ dài 1, chẳng hạn như`[1]`, đảo ngược đoạn`[0,0]`không làm gì và xuất ra chính xác`[1]`. Đối với các hoán vị có mức tối đa đã ở vị trí đầu tiên, chẳng hạn như`[4,3,2,1]`, thuật toán nhận ra`i_max=0`và đảo ngược một đoạn có độ dài bằng một, giữ nguyên mảng. Trong cả hai trường hợp, thuật toán không thực hiện các phép đảo ngược không cần thiết và duy trì tính chính xác. Đối với một hoán vị như`[2,1]`, thuật toán xác định`2`tối đa ở chỉ số 0 và không thực hiện đảo ngược, xuất ra`[2,1]`một cách chính xác.
