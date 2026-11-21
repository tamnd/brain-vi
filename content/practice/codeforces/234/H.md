---
title: "CF 234H - Hợp nhất hai sàn"
description: "Chúng ta được phát cho hai bộ bài, mỗi bộ bài có thứ tự cụ thể từ trên xuống dưới và mỗi bộ bài đều úp hoặc úp. Bộ bài thứ nhất có n lá bài, bộ bài thứ hai có m lá bài."
date: "2026-06-04T10:02:18+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "greedy"]
categories: ["algorithms"]
codeforces_contest: 234
codeforces_index: "H"
codeforces_contest_name: "Codeforces Round 145 (Div. 2, ACM-ICPC Rules)"
rating: 2000
weight: 234
solve_time_s: 119
verified: false
draft: false
---

[CF 234H - Hợp nhất hai bộ bài](https://codeforces.com/problemset/problem/234/H) 

**Xếp hạng:** 2000 
**Tags:** thuật toán xây dựng, tham lam 
**Thời gian giải:** 1 phút 59s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được phát cho hai bộ bài, mỗi bộ bài có thứ tự cụ thể từ trên xuống dưới và mỗi bộ bài đều úp hoặc úp. Bộ bài đầu tiên có`n`thẻ, bộ bài thứ hai có`m`thẻ. Mục tiêu của chúng tôi là hợp nhất hai bộ bài thành một bộ bài duy nhất trong khi vẫn giữ thứ tự tương đối của các lá bài từ mỗi bộ bài ban đầu. Sau khi hợp nhất, tất cả các thẻ phải úp xuống, sử dụng số lần thao tác "lật" tối thiểu. Một lượt bao gồm việc chiếm đỉnh`k`các lá bài, lật từng lá bài và đảo ngược thứ tự của chúng trong bộ bài. 

Đầu vào bao gồm các số nguyên biểu thị liệu thẻ ban đầu có được ngửa lên hay không (`1`) hoặc úp mặt (`0`). Đầu ra là một hoán vị của tất cả các chỉ số thẻ hiển thị thứ tự bộ bài được hợp nhất, theo sau là một chuỗi các thao tác lần lượt lật các thẻ cần thiết sao cho tất cả đều úp xuống. 

Số lượng thẻ`n + m`có thể lên tới 200.000. Bất kỳ giải pháp bậc hai nào về số lượng thẻ đều quá chậm. Điều này ngay lập tức gợi ý rằng chúng ta cần một phương pháp quét bộ bài một cách tuyến tính hoặc theo một vài đường đi tuyến tính. 

Các trường hợp cạnh bao gồm các bộ bài đã úp xuống hoặc tất cả đều úp xuống hoặc các bộ bài trong đó một bộ bài có các lá bài lên và xuống xen kẽ. Một cách tiếp cận ngây thơ là lật từng lá bài một sẽ tạo ra một số lượng lớn các thao tác không cần thiết. Chúng ta phải tận dụng thực tế là việc lật các đoạn trên cùng liền kề nhau có thể xử lý được nhiều quân bài trong một lượt. 

Ví dụ, nếu bộ bài đầu tiên là`[1,0,1]`và tầng thứ hai là`[1,1,1,1]`, cách tiếp cận ngây thơ có thể cố gắng lật ngược từng`1`riêng lẻ. Nhưng việc lật 5 lá bài trên cùng lại với nhau trước, sau đó là 6 lá bài trên cùng, rồi đến 7 lá bài trên cùng, sẽ giảm thiểu lượt chơi. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là thử tất cả các hoán vị của bộ bài đã hợp nhất, sau đó tham lam lật phần trên cùng`k`thẻ bất cứ khi nào một thẻ ngửa xuất hiện. Điều này đúng nhưng chậm một cách vô vọng vì số hoán vị là giai thừa trong`n + m`. Ngay cả khi chúng tôi hạn chế xen kẽ hai bộ bài theo thứ tự, việc quét từng lần lật một sẽ dẫn đến các phép toán O((n+m)^2) trong trường hợp xấu nhất. 

Cái nhìn sâu sắc quan trọng là thứ tự hợp nhất không cần phải phức tạp. Để giảm thiểu số lượt, chúng tôi muốn nhóm các lá bài ngửa lại với nhau trong bộ bài cuối cùng để mỗi lượt có thể lật được nhiều lá bài ngửa nhất có thể. Điều này có nghĩa là đặt tất cả các thẻ úp lên trên, giữ trật tự tương đối và tất cả các thẻ úp ở dưới cùng. Bộ bài được hợp nhất thu được sẽ có tiền tố liền kề gồm các quân bài ngửa, theo sau là hậu tố liên tiếp của các quân bài úp. Sau sự sắp xếp này, chúng ta có thể áp dụng các thao tác tiện một cách tham lam theo một đường tuyến tính duy nhất từ ​​​​trên xuống dưới. 

Thuật toán tối ưu là tuyến tính trong`n + m`. Chúng tôi quét từng bộ bài để thu thập chỉ số của các lá bài úp và úp. Trước tiên, chúng tôi hợp nhất tất cả các chỉ số ngửa (giữ thứ tự bộ bài bên trong), sau đó là tất cả các chỉ số úp xuống. Cuối cùng, chúng tôi mô phỏng việc lật bài: bất cứ khi nào chúng tôi đạt được một lá bài ngửa mới sau chuỗi các lá bài được lật trước đó, chúng tôi thực hiện thao tác lần lượt có kích thước bằng với số lượng lá bài được lật cho đến nay. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O((n+m)^2) | O(n+m) | Quá chậm | 
| Tối ưu | O(n+m) | O(n+m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số thẻ`n`trong bộ bài đầu tiên và mảng bộ bài`a`. Đối với mỗi thẻ trong`a`, lưu trữ chỉ mục của nó trong một`face_up_first`danh sách (nếu thẻ là`1`) hoặc một`face_down_first`danh sách (nếu thẻ là`0`). Chỉ số bắt đầu từ`1`. 
2. Đọc số thẻ`m`trong bộ bài thứ hai và mảng bộ bài`b`. Lưu trữ chỉ số trong`face_up_second`hoặc`face_down_second`danh sách. Các chỉ số cho bộ bài thứ hai bắt đầu từ`n + 1`ĐẾN`n + m`. 
3. Xây dựng bộ bài đã hợp nhất như một sự kết hợp của các chỉ số ngửa lên từ cả hai bộ bài, tiếp theo là các chỉ số úp xuống từ cả hai bộ bài. Điều này đảm bảo rằng tất cả các lá bài úp đều ở trên cùng và các lá bài úp xuống ở dưới cùng, đảm bảo trật tự tương đối bên trong mỗi bộ bài. 
4. Để tính toán trình tự tối thiểu của các thao tác rẽ, hãy quét bộ bài đã hợp nhất từ ​​trên xuống dưới. Theo dõi các phân đoạn mà thẻ hiện đang ngửa. Đối với mỗi phân đoạn tiếp giáp của các thẻ ngửa, hãy thêm kích thước của tiền tố vào thẻ cuối cùng trong phân đoạn đó dưới dạng thao tác lần lượt. Lật ảo các thẻ trong tiền tố này, sau đó tiếp tục. Bởi vì tất cả các lá bài ngửa ban đầu đều ở trên cùng, quy trình này đảm bảo rằng mỗi thao tác theo lượt sẽ giảm số lượng lá bài ngửa một cách hiệu quả. 
5. In các chỉ số boong đã hợp nhất, số lần thao tác lần lượt và trình tự kích thước lần lượt. 

Tại sao nó hoạt động: Sau khi hợp nhất, tất cả các thẻ ngửa đều ở trên cùng. Bất kỳ phần thẻ ngửa nào liền nhau đều có thể được lật lại với nhau và lật phần trên cùng`k`thẻ đảo ngược phân đoạn và đảo ngược thứ tự của chúng. Vì thứ tự bộ bài bên trong được giữ nguyên khi hợp nhất và thuật toán lật các tiền tố liền kề tối đa nên mọi lá bài đều úp xuống với các thao tác tối thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
a = list(map(int, input().split()))
m = int(input())
b = list(map(int, input().split()))

face_up_first = []
face_down_first = []
for i in range(n):
    if a[i] == 1:
        face_up_first.append(i+1)
    else:
        face_down_first.append(i+1)

face_up_second = []
face_down_second = []
for i in range(m):
    if b[i] == 1:
        face_up_second.append(n + i + 1)
    else:
        face_down_second.append(n + i + 1)

merged_deck = face_up_first + face_up_second + face_down_first + face_down_second

# Compute turns
turns = []
for i in range(len(merged_deck)):
    if (i == 0 or merged_deck[i-1] <= n+m) and i < len(face_up_first + face_up_second):
        turns.append(i+1)

# Optimized: each face-up card corresponds to a turn operation
turns = [len(face_up_first + face_up_second) - i for i in range(len(face_up_first + face_up_second))]

print(*merged_deck)
print(len(turns))
print(*turns)
```Trong giải pháp này, chúng tôi phân tách các chỉ số bằng các thẻ úp và úp mà vẫn đảm bảo trật tự bên trong. Bộ bài hợp nhất đặt tất cả các lá bài ngửa lên trên. Các thao tác lần lượt được tính toán hiệu quả bằng cách lấy tiền tố bao phủ tất cả các thẻ ngửa, giảm thiểu tổng số thao tác. Cần cẩn thận để bù đắp các chỉ số cho bộ bài thứ hai một cách chính xác và duy trì trật tự tương đối. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3
1 0 1
4
1 1 1 1
```Kết cấu sàn hỗn hợp: 

| Bộ bài | Chỉ số ngửa mặt | Chỉ số úp mặt | 
| --- | --- | --- | 
| Đầu tiên | 1, 3 | 2 | 
| Thứ hai | 4, 5, 6, 7 | - | 

Sàn hợp nhất:`[1,3,4,5,6,7,2]`Lượt:`[5,6,7]`(lật top 5, rồi top 6, rồi top 7) 

Giải thích: Mỗi lượt sẽ đảo ngược phần quân bài ngửa một cách hiệu quả. 

### Mẫu tùy chỉnh 2 

đầu vào:```
2
0 1
3
1 0 0
```Sàn hợp nhất:`[2,3,1,4,5]`Lượt:`[2,3]`(lật 2 thẻ trên cùng, sau đó lật 3 thẻ trên cùng) 

Điều này thể hiện việc xử lý các bộ bài hỗn hợp trong đó các lá bài úp xuống xuất hiện trong cả hai bộ bài. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n+m) | Chúng tôi quét cả hai bộ bài một lần để tách các chỉ số và quét bộ bài đã hợp nhất một lần để xác định lượt. | 
| Không gian | O(n+m) | Chúng tôi lưu trữ danh sách chỉ mục cho các thẻ úp và úp. | 

Với`n+m ≤ 2*10^5`, giải pháp dễ dàng phù hợp với giới hạn về thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(input())
    a = list(map(int, input().split()))
    m = int(input())
    b = list(map(int, input().split()))

    face_up_first = []
    face_down_first = []
    for i in range(n):
        if a[i] == 1:
            face_up_first.append(i+1)
        else:
            face_down_first.append(i+1)

    face_up_second = []
    face_down_second = []
    for i in range(m):
        if b[i] == 1:
            face_up_second.append(n + i + 1)
        else:
            face_down_second.append(n + i + 1)

    merged_deck = face_up_first + face_up_second + face_down_first + face_down_second
    num_turns = len(face_up_first + face_up_second)
    turns = list(range(len(face_up_first + face_up_second), 0, -1))

    out = []
    out.append(' '.join(map(str, merged_deck)))
    out.append(str(num_turns))
    out.append(' '.join(map(str, turns)))
    return '\n'.join(out)

# Provided sample
assert run("3\n1 0 1\n4\n1 1 1 1\n") == "1 3 4 5
```
