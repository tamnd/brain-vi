---
title: "CF 102961K - Thu Thập Số"
description: "Chúng ta được cho một dãy số nguyên biểu thị sự sắp xếp giống như hoán vị của các số riêng biệt. Nhiệm vụ là xác định cần bao nhiêu “vòng” để xử lý tất cả các số theo thứ tự tăng dần, trong đó mỗi vòng bao gồm việc quét chuỗi từ trái sang phải và chọn…"
date: "2026-07-04T06:52:27+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102961
codeforces_index: "K"
codeforces_contest_name: "CSES Problem Set: Sorting and Searching"
rating: 0
weight: 102961
solve_time_s: 38
verified: true
draft: false
---

[CF 102961K - Thu thập số](https://codeforces.com/problemset/problem/102961/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 38s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một dãy số nguyên biểu thị sự sắp xếp giống như hoán vị của các số riêng biệt. Nhiệm vụ là xác định cần bao nhiêu “vòng” để xử lý tất cả các số theo thứ tự tăng dần, trong đó mỗi vòng bao gồm việc quét chuỗi từ trái sang phải và chọn ra các số bất cứ khi nào chúng xuất hiện ở vị trí thứ tự bắt buộc tiếp theo. 

Một cách cụ thể hơn để nghĩ về điều này là chúng ta muốn đọc các giá trị từ 1 đến n theo thứ tự, nhưng chúng ta buộc phải duyệt mảng từ trái sang phải nhiều lần. Mỗi lần duyệt qua, chúng tôi chọn càng nhiều giá trị theo thứ tự tiếp theo càng tốt mà không bị lùi lại trong mảng. Khi chúng tôi không thể tiếp tục sử dụng thẻ hiện tại nữa, chúng tôi sẽ bắt đầu thẻ mới. 

Đầu vào cho n theo sau là một mảng có độ dài n chứa mỗi số nguyên từ 1 đến n đúng một lần. Đầu ra là một số nguyên duy nhất, số lần chuyển từ trái sang phải đầy đủ cần thiết để sử dụng chuỗi theo thứ tự tăng dần. 

Cấu trúc ràng buộc ngụ ý rằng n có thể lớn, thường lên tới 200.000 hoặc tương tự trong các bài toán kiểu Codeforces. Điều đó ngay lập tức loại trừ bất kỳ cách tiếp cận nào mô phỏng các lần truyền đầy đủ trên mảng nhiều lần. Một mô phỏng đơn giản quét mảng một lần cho mỗi số sẽ hoạt động giống như O(n²) trong trường hợp xấu nhất, quá chậm. 

Trường hợp cạnh tinh tế xuất hiện khi mảng đã được sắp xếp. Trong trường hợp đó, mọi thứ sẽ được tiêu thụ trong một lần duy nhất. Ngược lại là mảng được sắp xếp ngược, trong đó mỗi phần tử buộc phải vượt qua mới vì mọi số được yêu cầu tiếp theo đều xuất hiện sớm hơn số trước đó trong mảng. 

Ví dụ: nếu n = 5 và mảng là [5, 4, 3, 2, 1], chúng ta không thể chọn 1 cho đến khi hoàn tất lượt vượt qua, do đó, mỗi số đều yêu cầu lượt vượt qua riêng, dẫn đến đầu ra 5. Cách tiếp cận quét mỗi lượt đơn giản có thể vô tình tính toán lại tiến trình không chính xác nếu không đặt lại cẩn thận trạng thái giữa các lượt vượt qua. 

## Phương pháp tiếp cận 

Ý tưởng brute-force là mô phỏng quá trình theo đúng nghĩa đen. Chúng tôi duy trì giá trị mục tiêu hiện tại mà chúng tôi đang cố gắng chọn, bắt đầu từ 1. Chúng tôi liên tục quét mảng từ trái sang phải. Bất cứ khi nào chúng tôi gặp mục tiêu hiện tại, chúng tôi sẽ tăng mục tiêu. Khi đến cuối mảng, chúng tôi đếm một lần vượt qua và bắt đầu quét lại cho đến khi sử dụng hết tất cả các số. 

Điều này hiệu quả vì nó tuân theo định nghĩa của quy trình một cách trung thực. Vấn đề là chi phí. Trong trường hợp xấu nhất, mỗi lượt có thể chỉ tiêu thụ một phần tử, vì vậy chúng tôi quét toàn bộ mảng để tìm từng giá trị trong số n giá trị. Điều đó dẫn đến khoảng n lượt, mỗi lần lấy O(n), tạo ra tổng số thao tác O(n2). 

Quan sát quan trọng là điều quan trọng không phải là bản thân việc quét mà là ở chỗ các số liên tiếp xuất hiện tương đối với nhau trong mảng. Nếu số x+1 xuất hiện ở bên trái của x thì chúng tôi không thể tiếp tục vượt qua hiện tại khi đạt đến x, vì chúng tôi đã vượt qua x+1 trong lần quét này. Mỗi lần điều này xảy ra, chúng tôi buộc phải bắt đầu một thẻ mới. 

Vì vậy, thay vì mô phỏng các lượt chuyển, chúng tôi theo dõi vị trí của từng giá trị trong mảng. Chúng ta duyệt qua các giá trị từ 1 đến n và so sánh các vị trí. Bất cứ khi nào vị trí của giá trị hiện tại nhỏ hơn vị trí của giá trị trước đó, điều đó có nghĩa là thứ tự bị phá vỡ trong một lần quét và cần phải vượt qua mới. 

Điều này làm giảm vấn đề đếm số lần chuỗi vị trí giảm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng một mảng`pos`Ở đâu`pos[x]`lưu trữ chỉ số giá trị`x`trong hoán vị đã cho. Điều này chuyển đổi vấn đề từ việc làm việc trên chính mảng sang việc làm việc trên các vị trí, điều này làm cho việc so sánh diễn ra theo thời gian không đổi. 
2. Khởi tạo bộ đếm`rounds = 1`bởi vì luôn cần ít nhất một lượt để bắt đầu xử lý chuỗi. 
3. Lặp lại`x`từ 2 đến n, so sánh`pos[x]`với`pos[x-1]`. Mỗi phép so sánh kiểm tra xem liệu chúng ta có thể tiếp tục chuỗi tăng dần hiện tại trong cùng một lần quét hay không. 
4. Nếu`pos[x] < pos[x-1]`, tăng`rounds`bằng 1. Điều này có nghĩa là giá trị`x`xuất hiện trước`x-1`trong mảng, vì vậy khi quét từ trái sang phải chúng ta sẽ gặp phải`x-1`đầu tiên và do đó không thể xử lý`x`trong cùng một đường chuyền. 
5. Sau khi kết thúc vòng lặp, xuất ra`rounds`. 

### Tại sao nó hoạt động 

Thuật toán dựa trên tính bất biến là một lần quét chỉ có thể xử lý các giá trị có chỉ số xuất hiện theo thứ tự tăng dần. Mỗi lần chúng tôi phát hiện sự sụt giảm trong chuỗi chỉ mục của các giá trị liên tiếp, chúng tôi sẽ xác định ranh giới giữa hai lần quét. Vì các giá trị được tiêu thụ nghiêm ngặt theo thứ tự số tăng dần, nên mọi nghịch đảo giữa`pos[x]`Và`pos[x-1]`buộc phải thực hiện một hành trình mới. Số lần nghỉ như vậy khớp chính xác với số lần vượt qua được yêu cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input().strip())
a = list(map(int, input().split()))

pos = [0] * (n + 1)
for i, v in enumerate(a):
    pos[v] = i

rounds = 1
for x in range(2, n + 1):
    if pos[x] < pos[x - 1]:
        rounds += 1

print(rounds)
```Vòng lặp đầu tiên xây dựng bản đồ vị trí để mỗi giá trị biết vị trí của nó trong thời gian O(1). Điều này rất quan trọng vì nó biến việc quét lặp đi lặp lại thành những so sánh đơn giản. 

Vòng lặp thứ hai thực hiện quan sát cốt lõi: chúng tôi chỉ quan tâm đến việc liệu các giá trị liên tiếp có xuất hiện theo thứ tự chỉ số tăng dần hay không. Việc khởi tạo thành 1 phản ánh thực tế rằng ngay cả một mảng được sắp xếp hoàn hảo cũng cần ít nhất một lần vượt qua. 

Một lỗi phổ biến là khởi tạo`rounds`về 0 và cố gắng chỉ tăng khi nghỉ, điều này sẽ bị đếm thiếu một. Một vấn đề tinh vi khác là nhầm lẫn các giá trị với các chỉ số, việc so sánh phải được thực hiện giữa các vị trí của các giá trị chứ không phải giữa các giá trị. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
3 1 2 5 4
```Chúng tôi tính toán các vị trí: 

| x | vị trí[x] | 
| --- | --- | 
| 1 | 1 | 
| 2 | 2 | 
| 3 | 0 | 
| 4 | 4 | 
| 5 | 3 | 

Bây giờ chúng ta so sánh các giá trị liên tiếp: 

| x | vị trí[x-1] | vị trí[x] | Phá vỡ? | vòng | 
| --- | --- | --- | --- | --- | 
| 2 | 1 | 2 | Không | 1 | 
| 3 | 2 | 0 | Có | 2 | 
| 4 | 0 | 4 | Không | 2 | 
| 5 | 4 | 3 | Có | 3 | 

Đầu ra là 3. 

Dấu vết này cho thấy rằng mỗi khi thứ tự vị trí giảm thì cần phải có một đường chuyền mới, xác nhận ánh xạ giữa các lần đảo ngược và đặt lại quét. 

### Ví dụ 2 

đầu vào:```
4
1 2 3 4
```| x | vị trí[x-1] | vị trí[x] | Phá vỡ? | vòng | 
| --- | --- | --- | --- | --- | 
| 2 | 0 | 1 | Không | 1 | 
| 3 | 1 | 2 | Không | 1 | 
| 4 | 2 | 3 | Không | 1 | 

Đầu ra là 1. 

Điều này thể hiện trường hợp tốt nhất trong đó hoán vị đã được căn chỉnh theo thứ tự bắt buộc, do đó chỉ cần quét một lần là đủ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Một lần để xây dựng vị trí và một lần quét tuyến tính trên các giá trị | 
| Không gian | O(n) | Mảng lưu trữ vị trí của từng giá trị | 

Giải pháp phù hợp thoải mái trong các ràng buộc điển hình lên tới 200.000 phần tử. Cả thời gian và mức sử dụng bộ nhớ đều có quy mô tuyến tính, điều này tối ưu vì mọi phần tử phải được đọc ít nhất một lần. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    n = int(input().strip())
    a = list(map(int, input().split()))

    pos = [0] * (n + 1)
    for i, v in enumerate(a):
        pos[v] = i

    rounds = 1
    for x in range(2, n + 1):
        if pos[x] < pos[x - 1]:
            rounds += 1

    return str(rounds)

# provided samples
assert run("5\n3 1 2 5 4\n") == "3"

# minimum size
assert run("1\n1\n") == "1"

# already sorted
assert run("4\n1 2 3 4\n") == "1"

# reverse sorted
assert run("4\n4 3 2 1\n") == "4"

# random permutation
assert run("6\n4 1 3 2 6 5\n") == "4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 phần tử | 1 | trường hợp ranh giới tối thiểu | 
| mảng được sắp xếp | 1 | trường hợp không nghỉ | 
| mảng đảo ngược | n | sự phân mảnh tồi tệ nhất | 
| hoán vị hỗn hợp | 4 | tính đúng đắn chung | 

## Vỏ cạnh 

Một mảng một phần tử như`[1]`là kịch bản đơn giản nhất có thể. Mảng vị trí chỉ chứa một giá trị và vòng lặp trên các cặp liên tiếp không bao giờ chạy, để lại`rounds = 1`. Thuật toán xử lý chính xác điều này mà không cần sử dụng cách viết hoa đặc biệt. 

Một mảng được sắp xếp ngược lại như`[4, 3, 2, 1]`tạo ra các vị trí trong đó mỗi`pos[x] < pos[x-1]`đúng. Vòng lặp tăng dần`rounds`ở mỗi bước, dẫn đến 4 lần vượt qua. Điều này phù hợp với trực giác rằng mỗi số phải được xử lý theo một lần duyệt riêng biệt vì mọi giá trị bắt buộc tiếp theo sẽ xuất hiện sớm hơn trong mảng. 

Một trường hợp được sắp xếp một phần như`[3, 1, 2, 4]`tạo ra một lần ngắt khi di chuyển từ 2 đến 3 theo thứ tự vị trí. Thuật toán phát hiện chính xác một mức giảm và xuất ra 2, chia chính xác chuỗi thành hai lần quét.
