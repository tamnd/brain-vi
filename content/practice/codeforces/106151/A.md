---
title: "CF 106151A - zerorush"
description: "Chúng ta được cho một dãy số nguyên. Trong một lần di chuyển, chúng ta chọn bất kỳ phần tử đơn lẻ nào và thay đổi nó chính xác bằng cộng hai hoặc trừ hai. Chúng tôi lặp lại quá trình này cho đến khi ít nhất một phần tử trở thành chính xác bằng 0."
date: "2026-06-21T09:37:23+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106151
codeforces_index: "A"
codeforces_contest_name: "2025 ICPC Greek Collegiate Programming Contest (GRCPC 2025)"
rating: 0
weight: 106151
solve_time_s: 38
verified: true
draft: false
---

[CF 106151A - zerorush](https://codeforces.com/problemset/problem/106151/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 38s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một dãy số nguyên. Trong một lần di chuyển, chúng ta chọn bất kỳ phần tử đơn lẻ nào và thay đổi nó chính xác bằng cộng hai hoặc trừ hai. Chúng tôi lặp lại quá trình này cho đến khi ít nhất một phần tử trở thành chính xác bằng 0. Mục tiêu là giảm thiểu số lần di chuyển cần thiết để đạt được điều kiện đó hoặc báo cáo rằng điều đó là không thể. 

Điều quan trọng là chúng tôi không cố gắng chuyển đổi toàn bộ mảng thành bất kỳ thứ gì cụ thể. Chúng tôi chỉ quan tâm đến việc làm cho một vị trí đạt 0 nhanh nhất có thể và chúng tôi được phép tự do lựa chọn chỉ mục nào cần sửa đổi ở mỗi bước. 

Các ràng buộc là cực kỳ lớn, lên tới 10^6 số và giá trị có độ lớn lên tới 10^9. Điều này ngay lập tức loại trừ bất kỳ mô phỏng nào cố gắng theo dõi tất cả các giá trị trung gian hoặc khám phá các chuỗi di chuyển. Bất kỳ giải pháp nào cũng phải giảm vấn đề thành tính toán theo từng phần tử hoặc tổng hợp trong thời gian tuyến tính. 

Một điểm tinh tế là tính chẵn lẻ. Vì mọi thao tác đều thay đổi một số đi hai nên tính chẵn lẻ của mỗi phần tử không bao giờ thay đổi. Một số bắt đầu là số lẻ sẽ luôn là số lẻ và một số bắt đầu là số chẵn sẽ luôn là số chẵn. Vì số 0 là số chẵn nên bất kỳ số lẻ nào cũng không bao giờ có thể trở thành số 0 bất kể có áp dụng bao nhiêu phép toán. Điều này đã tạo ra cả một lớp các trường hợp không thể xảy ra. 

Trường hợp cạnh thứ hai xuất phát từ số âm. Báo cáo vấn đề cho phép các giá trị âm và đạt đến 0 từ một giá trị âm tuân theo các hạn chế tương tự về tính chẵn lẻ và kích thước bước. Một cách tiếp cận ngây thơ giả định tất cả các số đều có khoảng cách dương đến 0 sẽ thất bại nếu bỏ qua dấu hiệu đó không quan trọng, chỉ có khoảng cách tuyệt đối và tính chẵn lẻ. 

Ví dụ: nếu chúng ta có một mảng phần tử`[5]`, chúng ta có thể cố gắng giảm nó đi 2 lần: 5 → 3 → 1 → -1 → -3 ... Chúng ta không bao giờ đạt đến 0, vì vậy câu trả lời đúng là -1. Một giải pháp bất cẩn sử dụng`abs(a[i]) // 2`sẽ tuyên bố không chính xác rằng nó có thể về 0 sau 2 nước đi. 

Một ví dụ khác là`[4, 3]`. Mặc dù có thể đạt tới 4 trong 2 nước đi, nhưng 3 là không thể vì nó là số lẻ nên câu trả lời là -1. 

## Phương pháp tiếp cận 

Nếu chúng ta tập trung vào một yếu tố duy nhất, quá trình này sẽ trở nên đơn giản: chúng ta đang cố gắng đạt đến số 0 bằng cách sử dụng các bước có kích thước hai. Điều đó có nghĩa là chúng ta đang đi trên dòng số nguyên một cách hiệu quả với bước 2. 

Đối với một giá trị`x`, chúng ta chỉ có thể đạt được những số có cùng tính chẵn lẻ với`x`. Vì vậy, ứng cử viên duy nhất có thể đạt đến số 0 là số chẵn. Trong số đó, số lần di chuyển cần thiết chính xác là số lần chúng ta cần trừ hoặc cộng 2 để về 0, tức là`|x| / 2`. 

Vì chúng ta được phép thao tác trên bất kỳ chỉ mục nào ở mỗi bước nên chúng ta không cần lo lắng về sự tương tác giữa các phần tử. Mỗi nước đi được dành cho một phần tử duy nhất, vì vậy chiến lược tối ưu là chọn phần tử cần ít nước đi nhất để về 0. Tuy nhiên, bài toán yêu cầu số lần di chuyển tối thiểu cần thiết cho đến khi có ít nhất một phần tử trở thành số 0 chứ không phải tổng của tất cả các phần tử. Điều này có nghĩa là chúng tôi không hoàn thành tất cả các chuyển đổi, chỉ có lần truy cập thành công đầu tiên. 

Vì vậy, quá trình này chuyển sang tính toán, đối với mỗi phần tử, liệu nó có thể đạt đến 0 hay không và trong bao nhiêu lần di chuyển, sau đó lấy mức tối thiểu trên tất cả các ứng cử viên hợp lệ. 

Ý tưởng Brute-Force sẽ mô phỏng tất cả các chuỗi hoạt động, thử tất cả các lựa chọn ở mỗi bước. Mỗi bước có N lựa chọn và chúng ta có thể cần tới O(max |a_i|) bước, điều này hoàn toàn không khả thi. Yếu tố phân nhánh làm cho điều này theo cấp số nhân theo thời gian. 

Quan sát quan trọng là mỗi phần tử tiến hóa độc lập và việc đạt đến số 0 chỉ phụ thuộc vào tính chẵn lẻ và khoảng cách. Điều này thu gọn vấn đề thành một phép tính đơn giản cho từng phần tử. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | Hàm mũ | O(N) | Quá chậm | 
| Tính toán từng phần tử | O(N) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc mảng và khởi tạo một biến`answer`với vô cùng, vì chúng tôi muốn số lần di chuyển tối thiểu được yêu cầu trong số tất cả các phần tử hợp lệ. 
2. Đối với mỗi phần tử`x`, trước tiên hãy kiểm tra xem nó có chẵn không. Nếu như`x`là số lẻ, hãy bỏ qua nó ngay lập tức vì nó không bao giờ có thể đạt đến 0 dưới ±2 nước đi. 
3. Nếu`x`là số chẵn, hãy tính số bước di chuyển cần thiết để đạt đến số 0, tức là`|x| // 2`. Điều này thể hiện số bước chính xác ±2 cần thiết để giảm cường độ của nó xuống 0. 
4. Cập nhật`answer = min(answer, |x| // 2)`. 
5. Sau khi xử lý tất cả các phần tử, nếu không tìm thấy số chẵn, nghĩa là`answer`vẫn là vô cùng, trở về`-1`. Còn không thì trả lại`answer`. 

### Tại sao nó hoạt động 

Mỗi phần tử tiến hóa độc lập theo kích thước bước cố định là 2, do đó khả năng tiếp cận của nó về 0 chỉ phụ thuộc vào việc số 0 có nằm trong lớp dư lượng có thể tiếp cận theo modulo 2 hay không. Vì tính chẵn lẻ là bất biến trong phép toán nên các số lẻ bị ngắt kết nối vĩnh viễn khỏi 0. Đối với các số chẵn, mọi thao tác đều giảm khoảng cách tuyệt đối về 0 đúng 2 và không có cấu trúc phân nhánh nào có thể thay đổi sự tiến hóa khoảng cách này. Do đó, thời gian tối thiểu để đạt được điểm 0 được xác định đầy đủ cho từng yếu tố và câu trả lời chung đơn giản là câu trả lời tốt nhất trong số tất cả các ứng viên có thể tiếp cận riêng lẻ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    arr = list(map(int, input().split()))
    
    INF = 10**18
    ans = INF
    
    for x in arr:
        if x % 2 != 0:
            continue
        ans = min(ans, abs(x) // 2)
    
    if ans == INF:
        print(-1)
    else:
        print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai trực tiếp tuân theo quan sát rằng tính chẵn lẻ là điều kiện chặn duy nhất. Vòng lặp chỉ tính toán một câu trả lời ứng viên cho các giá trị chẵn. Việc sử dụng giá trị tuyệt đối đảm bảo xử lý chính xác các số âm, vì khoảng cách đến 0 là đối xứng ở cả hai phía. 

Giá trị trọng điểm`INF`được sử dụng để phát hiện trường hợp không tồn tại phần tử hợp lệ. Điều này là cần thiết vì việc trả về số 0 hoặc giá trị mặc định sẽ gợi ý thành công một cách không chính xác. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
N = 6
arr = [5, 4, 3, 4, 7]
```Chúng tôi xử lý từng phần tử: 

| x | chẵn lẻ | |x|//2 | tối thiểu hiện tại | 

|---|---|---|---| 

| 5 | lẻ | bỏ qua | INF | 

| 4 | thậm chí | 2 | 2 | 

| 3 | lẻ | bỏ qua | 2 | 

| 4 | thậm chí | 2 | 2 | 

| 7 | lẻ | bỏ qua | 2 | 

Câu trả lời cuối cùng là 2. 

Điều này chứng tỏ rằng chỉ có các phần tử chẵn mới đóng góp và chúng tôi đang lựa chọn một cách hiệu quả ứng cử viên số 0 có thể tiếp cận nhanh nhất. 

### Ví dụ 2 

đầu vào:```
N = 4
arr = [3, 11, 7, 13]
```| x | chẵn lẻ | |x|//2 | tối thiểu hiện tại | 

|---|---|---|---| 

| 3 | lẻ | bỏ qua | INF | 

| 11 | lẻ | bỏ qua | INF | 

| 7 | lẻ | bỏ qua | INF | 

| 13 | lẻ | bỏ qua | INF | 

Không có phần tử hợp lệ nào tồn tại nên câu trả lời là -1. 

Điều này khẳng định rằng chỉ tính chẵn lẻ mới quyết định tính khả thi, bất kể mức độ lớn như thế nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N) | Mỗi phần tử được xử lý một lần với công việc liên tục | 
| Không gian | O(1) | Chỉ có một số biến vô hướng được sử dụng | 

Giải pháp này dễ dàng xử lý N lên đến 10^6 vì nó thực hiện một bước tuyến tính duy nhất với số học đơn giản cho mỗi phần tử, trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# sample 1
assert run("5\n6 5 4 3 4 7\n") == "2"

# sample 2
assert run("4\n3 11 7 13\n") == "-1"

# single even element
assert run("1\n8\n") == "4"

# single odd element
assert run("1\n7\n") == "-1"

# mixed values
assert run("3\n2 6 10\n") == "1"

# large negative even
assert run("2\n-8 -3\n") == "4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| thậm chí độc thân | 4 | trường hợp tích cực cơ bản | 
| đơn lẻ | -1 | không thể | 
| hỗn hợp | 1 | lựa chọn tối thiểu | 
| thậm chí tiêu cực | 4 | tính đối xứng của giá trị tuyệt đối | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi tất cả các số đều là số lẻ. Trong tình huống này, không có chuỗi thao tác nào có thể tạo ra số 0, vì tính chẵn lẻ không bao giờ thay đổi. Thuật toán xử lý việc này một cách tự nhiên vì`ans`vẫn là vô cùng và chúng tôi trở lại`-1`. 

Một trường hợp cạnh khác là mảng một phần tử. Nếu giá trị là số chẵn thì câu trả lời đơn giản là`|x|/2`, nếu lẻ thì kết quả là`-1`. Thuật toán không yêu cầu bất kỳ cách viết hoa đặc biệt nào vì cả hai hành vi đều đã được mã hóa trong kiểm tra tính chẵn lẻ. 

Các giá trị âm hoạt động giống hệt với các giá trị dương vì mỗi thao tác ±2 sẽ dịch chuyển số một cách đối xứng xung quanh số 0. Thuật toán sử dụng đúng giá trị tuyệt đối, do đó`-8`Và`8`tạo ra kết quả tương tự, đó là 4 nước đi.
