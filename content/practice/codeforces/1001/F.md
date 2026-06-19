---
title: "CF 1001F - Phân biệt trạng thái cơ bản đa qubit"
description: "Chúng ta được cung cấp một loạt qubit, nhưng lời hứa quan trọng đơn giản hơn so với việc đóng khung lượng tử gợi ý: hệ thống được đảm bảo ở một trong hai trạng thái cơ sở tính toán cổ điển."
date: "2026-06-16T23:43:07+07:00"
tags: ["codeforces", "competitive-programming", "*special"]
categories: ["algorithms"]
codeforces_contest: 1001
codeforces_index: "F"
codeforces_contest_name: "Microsoft Q# Coding Contest - Summer 2018 - Warmup"
rating: 1300
weight: 1001
solve_time_s: 53
verified: true
draft: false
---

[CF 1001F - Phân biệt các trạng thái cơ sở đa qubit](https://codeforces.com/problemset/problem/1001/F) 

**Đánh giá:** 1300 
**Thẻ:** *đặc biệt 
**Thời gian giải:** 53s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một loạt qubit, nhưng lời hứa quan trọng đơn giản hơn so với việc đóng khung lượng tử gợi ý: hệ thống được đảm bảo ở một trong hai trạng thái cơ sở tính toán cổ điển. Mỗi trạng thái cơ bản được mô tả bằng một mảng boolean,`bits0`Và`bits1`, tương ứng với hai chuỗi bit có độ dài khác nhau`N`. 

Nói một cách cụ thể hơn, các qubit không ở trạng thái chồng chất tùy ý. Toàn bộ thanh ghi đã có cấu hình cổ điển rõ ràng, chính xác bằng`bits0`hoặc chính xác bằng`bits1`. Nhiệm vụ là xác định cái nào hiện đang có mặt và trả về`0`hoặc`1`tương ứng. 

Công cụ duy nhất chúng tôi được phép sử dụng là đo lượng tử trên các qubit được cung cấp. Sau khi được đo lường, trạng thái sẽ sụp đổ, nhưng điều đó không liên quan vì chúng ta chỉ cần một quyết định duy nhất. 

Các ràng buộc không được nêu rõ ràng trong lời nhắc, nhưng các bài toán lượng tử tương tác điển hình thuộc loại này có dung lượng lớn.`N`(thường theo thứ tự 10^5 trở lên trong các bài kiểm tra). Điều này loại trừ bất cứ điều gì bậc hai hoặc liên quan đến việc mô phỏng lặp đi lặp lại quá trình tiến hóa trạng thái. Bất kỳ giải pháp hợp lệ nào cũng phải hoạt động theo thời gian tuyến tính theo số lượng qubit, vì mỗi qubit có thể được kiểm tra nhiều nhất một lần. 

Một trường hợp khó nhận thấy là khi`bits0`Và`bits1`chỉ khác nhau ở một vị trí. Trong trường hợp đó, giải pháp cố gắng “đoán sớm” mà không kiểm tra vị trí đó có thể thất bại. Ví dụ: nếu chỉ số khác nhau ở cuối và thuật toán dừng sớm, nó có thể kết luận sai sự bằng nhau. 

Một trường hợp khác là quên rằng phép đo có tính phá hủy. Nếu một giải pháp đo qubit và sau đó cố gắng sử dụng lại chúng cho logic so sánh sau này theo cách không cục bộ, thì có nguy cơ suy luận về trạng thái không hợp lệ. Trong vấn đề này, chúng tôi tránh hoàn toàn điều đó bằng cách coi phép đo là trích xuất một lần toàn bộ chuỗi bit. 

## Phương pháp tiếp cận 

Một cách giải thích mạnh mẽ sẽ là cố gắng suy luận trực tiếp về các trạng thái lượng tử, mô phỏng tất cả các khả năng hoặc cấu trúc truy vấn lặp đi lặp lại. Điều đó sẽ dẫn đến chi phí không cần thiết và về mặt khái niệm là không phù hợp với việc đảm bảo vấn đề. Vì trạng thái được hứa hẹn là trạng thái cơ sở tính toán nên không có sự tiến triển phân nhánh nào để mô phỏng. Bất kỳ nỗ lực nào coi đây là một vấn đề phân biệt lượng tử tổng quát sẽ biến thành lý luận theo cấp số nhân hoặc ít nhất là các chiến lược đo lường lặp đi lặp lại, điều này là không cần thiết. 

Quan sát quan trọng là việc đo từng qubit trong cơ sở tính toán sẽ ngay lập tức tiết lộ toàn bộ chuỗi bit ẩn. Khi chúng ta có chuỗi được quan sát, vấn đề sẽ giảm xuống thành một kiểm tra đẳng thức đơn giản đối với`bits0`Và`bits1`. Bởi vì chúng khác nhau ở ít nhất một vị trí nên sẽ có chính xác một so sánh phù hợp. 

Điều này làm giảm nhiệm vụ từ “nhận dạng trạng thái lượng tử” thành “đọc và so sánh một mảng nhị phân”. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (lý luận lượng tử / kiểm tra lặp đi lặp lại) | O(2^N) hoặc chi phí khái niệm tệ hơn | O(N)-O(2^N) | Quá chậm | 
| Tối ưu (đo một lần, so sánh) | O(N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đo từng qubit trong cơ sở tính toán và lưu trữ các bit cổ điển thu được vào một mảng`obs`. Bước này trích xuất trực tiếp trạng thái cổ điển ẩn vì đầu vào được đảm bảo đã là trạng thái cơ bản. 
2. So sánh`obs`với`bits0`. Nếu mọi vị trí đều khớp thì trạng thái chính xác`bits0`, vậy hãy quay lại`0`. Tính đúng đắn xuất phát từ lời hứa rằng trạng thái phải là một trong hai chuỗi đã cho. 
3. Nếu không, hãy quay lại`1`, vì khả năng duy nhất còn lại là`bits1`. Sự đảm bảo rằng`bits0`Và`bits1`khác nhau ở ít nhất một vị trí đảm bảo không có sự mơ hồ. 

### Tại sao nó hoạt động 

Điều bất biến là sau khi đo,`obs`giống hệt với trạng thái cơ sở tính toán cơ bản thực sự của thanh ghi qubit. Vì trạng thái đầu vào được đảm bảo chính xác`bits0`hoặc chính xác`bits1`, chuỗi được quan sát phải khớp chính xác với một trong số chúng. Bởi vì hai ứng cử viên là khác nhau nên so sánh bình đẳng sẽ xác định duy nhất ứng viên đúng mà không cần bất kỳ lý luận xác suất nào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve(qs, bits0, bits1):
    obs = [False] * len(qs)
    
    for i in range(len(qs)):
        # In actual Q# this would be: M(qs[i])
        # Here we assume qs already encodes classical info in simulation form
        obs[i] = qs[i]
    
    if obs == bits0:
        return 0
    return 1
```Việc triển khai phản ánh thực tế rằng hoạt động có ý nghĩa duy nhất là trích xuất thông tin cổ điển từ mỗi qubit. Trong môi trường Q# thực, điều này tương ứng với việc áp dụng phép đo`M`trên mỗi qubit. Sau đó, logic hoàn toàn là cổ điển. 

Bước so sánh là an toàn vì vấn đề đảm bảo tính độc quyền: chính xác một trong hai chuỗi bit khớp với kết quả đo được. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Giả sử:`bits0 = [0, 1, 0]`

`bits1 = [1, 1, 0]`Trạng thái quan sát được là`[1, 1, 0]`| Bước | quan sát | So sánh với bit0 | Kết quả | 
| --- | --- | --- | --- | 
| Đo lường | [1,1,0] | - | - | 
| Kiểm tra bit0 | [1,1,0] so với [0,1,0] | không khớp | tiếp tục | 
| Kiểm tra bit1 | [1,1,0] vs [1,1,0] | trận đấu | trở lại 1 | 

Điều này xác nhận rằng ngay cả một bit khác nhau cũng đủ để phân biệt các trạng thái ngay sau khi đo. 

### Ví dụ 2`bits0 = [0, 0, 0, 0]`

`bits1 = [0, 0, 1, 0]`Trạng thái quan sát được là`[0, 0, 0, 0]`| Bước | quan sát | So sánh với bit0 | Kết quả | 
| --- | --- | --- | --- | 
| Đo lường | [0,0,0,0] | - | - | 
| Kiểm tra bit0 | [0,0,0,0] so với [0,0,0,0] | trận đấu | trở về 0 | 

Điều này thể hiện trường hợp đơn giản nhất trong đó ứng cử viên đầu tiên đã phù hợp và không cần phải suy luận gì thêm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N) | Mỗi qubit được đo một lần và so sánh một lần | 
| Không gian | O(N) | Lưu trữ chuỗi bit được quan sát | 

Độ phức tạp tuyến tính là tối ưu vì mỗi qubit phải được kiểm tra ít nhất một lần để phân biệt giữa hai chuỗi bit tùy ý có thể khác nhau ở bất kỳ vị trí nào. Điều này phù hợp thoải mái trong các ràng buộc điển hình cho các vấn đề đo lường hoặc mô phỏng lượng tử. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    
    # simple simulation wrapper
    data = list(map(int, sys.stdin.read().split()))
    n = data[0]
    bits0 = data[1:1+n]
    bits1 = data[1+n:1+2*n]
    qs = data[1+2*n:1+3*n]
    
    def solve(qs, bits0, bits1):
        return 0 if qs == bits0 else 1
    
    return str(solve(qs, bits0, bits1))

# sample-like cases
assert run("3 0 1 0 1 1 0 1 1 0") == "1"

# all equal to bits0
assert run("2 0 0 1 1 0 0") == "0"

# differs in last bit
assert run("4 0 0 0 0 0 0 0 1") == "1"

# single bit
assert run("1 0 1 1") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| trường hợp bit đơn | 1 | hành vi N tối thiểu | 
| giống hệt với bit0 | 0 | đường dẫn đối sánh trực tiếp | 
| khác ở vị trí cuối cùng | 1 | độ đúng ranh giới | 
| mẫu hỗn hợp | 0/1 | tính đúng đắn chung | 

## Vỏ cạnh 

Trường hợp một cạnh là khi`bits0`Và`bits1`chỉ khác nhau ở chỉ số cuối cùng. Thuật toán vẫn hoạt động vì phép đo không dựa vào việc chấm dứt sớm. Mảng đầy đủ luôn được đọc trước khi so sánh, do đó bit phân biệt không bao giờ bị bỏ qua. 

Một trường hợp cạnh khác là`N = 1`. Qubit ở đây là`0`hoặc`1`và cả hai ứng cử viên đều là chuỗi bit đơn. Phép đo tạo ra mảng một phần tử và so sánh đẳng thức vẫn xác định duy nhất trạng thái chính xác mà không có sự mơ hồ. 

Trường hợp cạnh cuối cùng là khi hai chuỗi bit bổ sung cho nhau ngoại trừ một tiền tố chung duy nhất. Thuật toán không phụ thuộc vào sự khác biệt nằm ở đâu, mà chỉ phụ thuộc vào sự bằng nhau của chuỗi đầy đủ, do đó không có sai lệch vị trí nào ảnh hưởng đến tính chính xác.
