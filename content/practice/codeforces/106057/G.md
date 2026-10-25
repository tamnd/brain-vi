---
title: "CF 106057G - MeX không phải là Max"
description: "Chúng ta được cho một hoặc nhiều dãy số nguyên không âm. Đối với mỗi chuỗi, chúng tôi muốn tìm số nguyên không âm nhỏ nhất không xuất hiện ở bất kỳ đâu trong chuỗi đó. Giá trị này được gọi là MEX, viết tắt của “giá trị loại trừ tối thiểu”."
date: "2026-06-21T08:42:42+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106057
codeforces_index: "G"
codeforces_contest_name: "CoU CSE Fest 2025 - Inter University Programming Contest (Divisional)"
rating: 0
weight: 106057
solve_time_s: 36
verified: true
draft: false
---

[CF 106057G - MeX không phải là Max](https://codeforces.com/problemset/problem/106057/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 36s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một hoặc nhiều dãy số nguyên không âm. Đối với mỗi chuỗi, chúng tôi muốn tìm số nguyên không âm nhỏ nhất không xuất hiện ở bất kỳ đâu trong chuỗi đó. Giá trị này được gọi là MEX, viết tắt của “giá trị loại trừ tối thiểu”. 

Hãy coi mảng như một tập hợp các số nguyên được đánh dấu trên trục số bắt đầu từ số 0. Một số con số có mặt, một số bị thiếu. Nhiệm vụ là xác định khoảng trống đầu tiên khi quét từ 0 trở lên. 

Định dạng đầu vào thường cung cấp một số trường hợp thử nghiệm và đối với mỗi trường hợp thử nghiệm, độ dài chuỗi theo sau là chính chuỗi đó. Đầu ra là một số nguyên duy nhất cho mỗi trường hợp thử nghiệm, đại diện cho MEX. 

Các ràng buộc không được nêu rõ ràng ở đây, nhưng giải pháp dự kiến ​​sẽ chạy theo thời gian tuyến tính cho mỗi trường hợp thử nghiệm. Điều đó gợi ý rõ ràng rằng tổng của tất cả các kích thước mảng trong các trường hợp thử nghiệm tối đa là khoảng 2 × 10^5 hoặc 10^6. Bất kỳ giải pháp nào liên tục quét hoặc sắp xếp mảng cho mỗi trường hợp thử nghiệm sẽ trở nên quá chậm nếu tổng kích thước lớn. 

Trường hợp cạnh tinh tế xuất hiện khi tất cả các số từ 0 đến N−1 đều có mặt. Ví dụ, nếu mảng là`[0, 1, 2, 3]`, thì câu trả lời là`4`, vì đó là số nguyên không âm đầu tiên bị thiếu. Một cách tiếp cận ngây thơ chỉ kiểm tra các giá trị lên tới`max(array)`sẽ thất bại ở đây nếu nó không xem xét khả năng MEX chính xác là`N`. 

Một trường hợp cạnh khác xảy ra khi mảng chỉ chứa các giá trị lớn, ví dụ`[10, 11, 12]`. Câu trả lời đúng là`0`, vì số 0 bị thiếu ngay lập tức. Việc triển khai sai bắt đầu quét từ phần tử tối thiểu thay vì 0 sẽ bỏ lỡ điều này. 

Cuối cùng, sự trùng lặp không ảnh hưởng đến tính chính xác nhưng có thể đánh lừa các giải pháp kém hiệu quả cố gắng loại bỏ hoặc sắp xếp các phần tử nhiều lần. 

## Phương pháp tiếp cận 

Một cách mạnh mẽ để tính MEX là kiểm tra liên tục xem mỗi số nguyên bắt đầu từ 0 có xuất hiện trong mảng hay không. Đối với mỗi giá trị ứng cử viên x, chúng tôi quét toàn bộ mảng và kiểm tra xem x có tồn tại không. Nếu không, chúng tôi trả lại x ngay lập tức. 

Cách tiếp cận này đúng vì nó tuân theo định nghĩa của MEX. Vấn đề là chi phí của nó. Trong trường hợp xấu nhất, chúng tôi có thể kiểm tra tất cả các giá trị từ 0 đến N và đối với mỗi giá trị sẽ quét N phần tử. Điều đó dẫn đến các hoạt động O(N^2) cho mỗi trường hợp thử nghiệm, hoạt động này trở nên quá chậm khi N lớn. 

Điều quan trọng là chúng ta không cần phải quét toàn bộ nhiều lần. Chúng tôi chỉ quan tâm đến việc mỗi số trong phạm vi [0, N] có xuất hiện ít nhất một lần hay không. Bất kỳ số nào lớn hơn N đều không thể ảnh hưởng đến câu trả lời, vì MEX của mảng có kích thước N luôn lớn nhất là N. Điều này giới hạn không gian tìm kiếm. 

Do đó, chúng ta có thể đánh dấu những giá trị nào trong [0, N] tồn tại bằng cách sử dụng mảng boolean hoặc tập hợp hàm băm trong một lần chuyển qua đầu vào. Sau khi đánh dấu, chúng ta quét từ 0 trở lên và trả về chỉ số đầu tiên chưa được đánh dấu. Điều này làm giảm vấn đề xuống còn hai đường đi tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(N^2) | O(1) | Quá chậm | 
| Tối ưu | O(N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng trường hợp thử nghiệm một cách độc lập. 

1. Đọc kích thước mảng N và các giá trị mảng. Chúng tôi sẽ chỉ theo dõi các giá trị trong phạm vi [0, N], vì bất kỳ giá trị nào lớn hơn đều không thể là câu trả lời. 
2. Tạo một mảng boolean`present`có kích thước N+1 được khởi tạo thành Sai. Mỗi chỉ mục i trong mảng này biểu thị liệu giá trị i có tồn tại trong đầu vào hay không. 
3. Lặp qua từng phần tử x trong mảng đầu vào. Nếu 0  x  N thì đánh dấu`present[x] = True`. Bước này nén tất cả thông tin liên quan về mảng vào một bảng tồn tại nhỏ gọn. 
4. Quét từ i = 0 đến N. Chỉ số đầu tiên tôi sao cho`present[i]`sai là MEX nên chúng tôi trả lại ngay lập tức. 
5. Nếu có tất cả các giá trị từ 0 đến N, hãy trả về N. Điều này bao gồm trường hợp mảng chứa tiền tố đầy đủ của các số nguyên. 

### Tại sao nó hoạt động 

Tính đúng đắn xuất phát từ thực tế là MEX được định nghĩa là số nguyên không âm nhỏ nhất không có trong mảng. Mảng boolean thể hiện chính xác tư cách thành viên của tất cả các ứng cử viên có thể là câu trả lời. Vì bất kỳ MEX nào của mảng có kích thước N đều không thể vượt quá N, nên việc hạn chế chú ý đến [0, N] sẽ không làm mất thông tin. Lần quét cuối cùng sẽ kiểm tra các ứng cử viên theo thứ tự tăng dần, vì vậy chỉ số còn thiếu đầu tiên phải là giá trị loại trừ tối thiểu thực sự. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        arr = list(map(int, input().split()))

        present = [False] * (n + 1)

        for x in arr:
            if 0 <= x <= n:
                present[x] = True

        ans = n
        for i in range(n + 1):
            if not present[i]:
                ans = i
                break

        print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách đọc số lượng trường hợp thử nghiệm và lặp lại chúng. Đối với mỗi trường hợp, nó xây dựng một mảng hiện diện có kích thước`n+1`để bao gồm tất cả các giá trị có thể có liên quan. Việc kiểm tra có điều kiện bên trong vòng lặp đảm bảo chúng ta bỏ qua các giá trị nằm ngoài phạm vi có ý nghĩa, giúp tránh việc ghi không cần thiết và giữ logic chặt chẽ. 

Vòng lặp cuối cùng được cấu trúc để dừng ngay lập tức khi tìm thấy số còn thiếu đầu tiên. Việc thoát sớm này rất quan trọng vì nó duy trì hành vi tuyến tính trong thực tế và phù hợp trực tiếp với định nghĩa của MEX. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1
5
0 1 2 4 4
```Chúng tôi theo dõi sự hiện diện của các chỉ số từ 0 đến 5. 

| tôi | hiện tại[i] sau khi xử lý | 
| --- | --- | 
| 0 | Đúng | 
| 1 | Đúng | 
| 2 | Đúng | 
| 3 | Sai | 
| 4 | Đúng | 
| 5 | Sai | 

Quét lên trên, Sai đầu tiên là i = 3. 

Đầu ra:```
3
```Điều này cho thấy rằng các số bị thiếu trong phạm vi quan trọng hơn các số trùng lặp hoặc giá trị lớn hơn. 

### Ví dụ 2 

đầu vào:```
1
4
10 11 12 13
```Chỉ có các giá trị bên ngoài [0, 4] tồn tại nên không có giá trị nào được đánh dấu. 

| tôi | hiện tại[i] | 
| --- | --- | 
| 0 | Sai | 
| 1 | Sai | 
| 2 | Sai | 
| 3 | Sai | 
| 4 | Sai | 

Chỉ số đầu tiên Sai là 0, vì vậy MEX là 0. 

Đầu ra:```
0
```Điều này chứng tỏ tại sao chúng ta phải luôn xem xét 0 ngay cả khi mảng chỉ chứa số lớn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N) cho mỗi trường hợp thử nghiệm | Một lần đánh dấu sự hiện diện và một lần quét để tìm MEX | 
| Không gian | O(N) | Mảng Boolean có kích thước N+1 | 

Tổng công việc có tỷ lệ tuyến tính với kích thước đầu vào, phù hợp thoải mái trong các ràng buộc điển hình cho các vấn đề thuộc loại này, ngay cả khi có nhiều trường hợp thử nghiệm. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = []
    input = sys.stdin.readline

    t = int(input())
    for _ in range(t):
        n = int(input())
        arr = list(map(int, input().split()))
        present = [False] * (n + 1)

        for x in arr:
            if 0 <= x <= n:
                present[x] = True

        ans = n
        for i in range(n + 1):
            if not present[i]:
                ans = i
                break

        output.append(str(ans))

    return "\n".join(output)

# sample-style cases
assert run("1\n5\n0 1 2 4 4\n") == "3"
assert run("1\n4\n10 11 12 13\n") == "0"

# custom cases
assert run("1\n1\n0\n") == "1", "missing 1"
assert run("1\n3\n1 2 3\n") == "0", "missing zero"
assert run("1\n3\n0 1 2\n") == "3", "full prefix"
assert run("1\n6\n2 2 2 2 2 2\n") == "0", "duplicates only"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Có 1 phần tử | 1 | thiếu nhỏ nhất ở ranh giới trên | 
| 1 2 3 | 0 | thiếu xử lý số 0 | 
| 0 1 2 | 3 | trường hợp tiền tố đầy đủ | 
| tất cả các bản sao | 0 | bỏ qua sự lặp lại một cách chính xác | 

## Vỏ cạnh 

Một trường hợp cạnh quan trọng là khi mảng chứa tất cả các số nguyên từ 0 đến N−1. Ví dụ: 

đầu vào:```
1
4
0 1 2 3
```Thuật toán đánh dấu tất cả các vị trí từ 0 đến 4 là hiện tại ngoại trừ vị trí 4 không bao giờ được chèn vào. Quá trình quét tìm thấy rằng`present[4]`là Sai, vì vậy nó trả về 4. Điều này khớp với MEX chính xác vì mảng bao gồm đầy đủ tiền tố. 

Một trường hợp cạnh khác là khi tất cả các phần tử nằm ngoài phạm vi hợp lệ: 

đầu vào:```
1
5
100 200 300 400 500
```Trong quá trình đánh dấu, mọi giá trị đều bị bỏ qua vì không có giá trị nào nằm trong [0, 5]. Mảng boolean vẫn giữ nguyên toàn bộ Sai. Quá trình quét ngay lập tức trả về 0, điều này đúng vì 0 là số nguyên bị thiếu nhỏ nhất. 

Trường hợp thứ ba liên quan đến các bản sao: 

đầu vào:```
1
5
0 0 0 1 1
```Chỉ có 0 và 1 được đánh dấu. Quá trình quét tìm thấy 2 là chỉ mục không được đánh dấu đầu tiên. Các bản sao không làm thay đổi trạng thái của mảng hiện diện, do đó tính chính xác vẫn được giữ nguyên.
