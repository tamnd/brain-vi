---
title: "CF 106059L - Lễ hội đèn lồng"
description: "Vấn đề cơ bản là yêu cầu chúng ta xử lý một hàng đèn lồng dọc theo bờ sông, tại đó mỗi chiếc đèn lồng đều bật hoặc tắt. Dữ liệu đầu vào cung cấp cho chúng ta một chuỗi có độ dài n và mỗi vị trí chứa số 0 nghĩa là đèn lồng không sáng hoặc số 1 nghĩa là nó đang phát sáng."
date: "2026-06-22T18:42:04+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106059
codeforces_index: "L"
codeforces_contest_name: "National Yang Ming Chiao Tung University 2025 Team Selection Programming Contest"
rating: 0
weight: 106059
solve_time_s: 46
verified: true
draft: false
---

[CF 106059L - Lễ hội đèn lồng](https://codeforces.com/problemset/problem/106059/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 46s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Vấn đề cơ bản là yêu cầu chúng ta xử lý một hàng đèn lồng dọc theo bờ sông, tại đó mỗi chiếc đèn lồng đều bật hoặc tắt. Dữ liệu đầu vào cung cấp cho chúng ta một chuỗi có độ dài n và mỗi vị trí chứa số 0 nghĩa là đèn lồng không sáng hoặc số 1 nghĩa là nó đang phát sáng. Nhiệm vụ là xác định xem hiện tại có bao nhiêu chiếc đèn lồng đang thắp sáng, tương đương với việc đếm xem có bao nhiêu giá trị trong dãy bằng 1. 

Mặc dù tuyên bố này đóng khung điều này trong bối cảnh lễ hội, nhưng không có gì quan trọng về vị trí, sự liền kề hoặc cấu trúc ngoài các giá trị thô. Trình tự này độc lập và mỗi phần tử góp phần đưa ra câu trả lời mà không tương tác với phần tử khác. 

Ràng buộc n lên tới 2 × 10^5 cho chúng ta biết rằng chúng ta phải quét mảng theo thời gian tuyến tính. Bất kỳ cách tiếp cận nào liên tục xử lý các phần tử giống nhau hoặc sử dụng các vòng lặp lồng nhau trên mảng sẽ quá chậm. Giải pháp bậc hai sẽ bao hàm khoảng 4 × 10^10 phép tính trong trường hợp xấu nhất, vượt xa giới hạn thời gian. Điều này ngay lập tức gợi ý rằng tích lũy một lượt là hướng đi chính xác. 

Các trường hợp cạnh ở đây chủ yếu liên quan đến định dạng đầu vào và kích thước ranh giới. Nếu n = 1, chúng ta chỉ cần trả về giá trị đơn đó. Nếu tất cả các giá trị là 0 thì câu trả lời là 0. Nếu tất cả các giá trị là 1 thì câu trả lời là n. Một vấn đề nhỏ có thể xuất hiện trong quá trình triển khai là giả sử dòng thứ hai luôn được định dạng rõ ràng hoặc chứa chính xác n số nguyên cách nhau bằng dấu cách, nhưng quên rằng đầu vào có thể trải dài trên các dòng lớn yêu cầu phân tích cú pháp nhanh. 

## Phương pháp tiếp cận 

Một cách đơn giản để giải quyết vấn đề là lặp trực tiếp qua danh sách các trạng thái đèn lồng và đếm xem có bao nhiêu bằng 1. Cách này hiệu quả vì mỗi đèn lồng đóng góp độc lập vào tổng số cuối cùng. Chúng tôi không cần sắp xếp, cấu trúc tiền tố hoặc bất kỳ hình thức tiền xử lý nào vì không có truy vấn phạm vi hoặc phụ thuộc giữa các vị trí. 

Cách giải thích bạo lực vẫn có thể được diễn đạt là kiểm tra từng chiếc đèn lồng riêng lẻ và tăng bộ đếm khi nó được thắp sáng. Thế là đã tối ưu về cơ cấu rồi. Mọi nỗ lực tính toán lại số lượng lặp đi lặp lại, ví dụ như tính toán lại tổng theo tiền tố cho từng vị trí truy vấn, sẽ không cần thiết vì vấn đề chỉ yêu cầu một số lượng tổng thể duy nhất. 

Quan sát quan trọng là cấu trúc của đầu vào đã ở mức tối thiểu. Không cần chuyển đổi, chỉ cần tổng hợp. Điều này làm giảm vấn đề tính tổng của một mảng nhị phân, vì mỗi số 1 đóng góp chính xác một đơn vị cho câu trả lời và mỗi số 0 không đóng góp gì cả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (tính toán lại hoặc đếm lồng nhau) | O(n) hoặc tệ hơn O(n^2) | O(1) | Quá chậm nếu thực hiện công việc lặp đi lặp lại | 
| Tối ưu (tổng một lần) | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi quét trình tự một lần trong khi duy trì tổng số đèn lồng đang hoạt động. 

1. Đọc n, số lượng đèn lồng. Điều này xác định số lượng giá trị chúng tôi sẽ xử lý và đảm bảo chúng tôi dừng chính xác sau khi sử dụng chuỗi. 
2. Khởi tạo bộ đếm về 0. Bộ đếm này thể hiện số lượng đèn lồng đã được thắp sáng cho đến nay. 
3. Lặp lại từng giá trị trong số n giá trị trong chuỗi đầu vào. Mỗi giá trị đại diện cho trạng thái của một chiếc đèn lồng. 
4. Đối với mỗi giá trị, hãy kiểm tra xem nó có bằng 1 hay không. Nếu có, hãy tăng bộ đếm lên một. Nếu là 0 thì không làm gì cả. Bước này hiệu quả vì mỗi chiếc đèn lồng đóng góp độc lập và chỉ những chiếc đèn lồng được thắp sáng mới là yếu tố quan trọng trong lần đếm cuối cùng. 
5. Sau khi xử lý tất cả các giá trị, xuất bộ đếm. Tại thời điểm này, nó chứa tổng số đèn lồng đã được thắp sáng trong chuỗi đầu vào. 

### Tại sao nó hoạt động

Thuật toán duy trì bất biến rằng sau khi xử lý k phần tử đầu tiên, bộ đếm bằng số 1 trong số k phần tử đó. Mỗi bước duy trì tính bất biến này vì cách duy nhất để thay đổi số lượng đèn lồng thực sự được thắp sáng trong tiền tố là bao gồm phần tử hiện tại nếu nó bằng 1. Vì mỗi phần tử được xử lý chính xác một lần và đóng góp chính xác một lần khi và chỉ khi nó là 1, nên bộ đếm cuối cùng bằng tổng số đèn lồng được thắp sáng trong toàn bộ chuỗi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input().strip())
    arr = list(map(int, input().split()))
    
    cnt = 0
    for x in arr:
        cnt += x
    
    print(cnt)

if __name__ == "__main__":
    solve()
```Giải pháp đọc kích thước n và sau đó phân tích toàn bộ dòng thứ hai thành một danh sách số nguyên. Vì các giá trị chỉ là 0 hoặc 1 nên việc tính tổng trực tiếp chúng tương đương với việc đếm số đèn lồng đang thắp sáng, vừa nhỏ gọn vừa hiệu quả. 

Một lỗi thường gặp là quên tách hoặc phân chia chính xác dòng đầu vào, đặc biệt khi n lớn. sử dụng`sys.stdin.readline`đảm bảo đọc nhanh. Một điểm tinh tế khác là việc tính tổng các số nguyên ở đây an toàn vì các giá trị nhỏ và không có nguy cơ tràn trong Python. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 7
arr = [1, 0, 1, 1, 0, 0, 1]
```| Bước | Giá trị hiện tại | Quầy | 
| --- | --- | --- | 
| 1 | 1 | 1 | 
| 2 | 0 | 1 | 
| 3 | 1 | 2 | 
| 4 | 1 | 3 | 
| 5 | 0 | 3 | 
| 6 | 0 | 3 | 
| 7 | 1 | 4 | 

Đầu ra cuối cùng là 4. 

Dấu vết này cho thấy mỗi giá trị 1 đóng góp chính xác một mức tăng như thế nào, trong khi các giá trị 0 không giữ nguyên trạng thái. Tổng số hoạt động khớp với số lượng đèn lồng thắp sáng ở mỗi tiền tố. 

### Ví dụ 2 

đầu vào:```
n = 5
arr = [0, 1, 0, 0, 1]
```| Bước | Giá trị hiện tại | Quầy | 
| --- | --- | --- | 
| 1 | 0 | 0 | 
| 2 | 1 | 1 | 
| 3 | 0 | 1 | 
| 4 | 0 | 1 | 
| 5 | 1 | 2 | 

Đầu ra cuối cùng là 2. 

Điều này xác nhận rằng thuật toán không phụ thuộc vào vị trí hoặc mẫu thứ tự. Chỉ có số lượng số 1 là quan trọng và sự phân bổ của chúng là không liên quan. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi chiếc đèn lồng được xử lý chính xác một lần trong một lần | 
| Không gian | O(1) | Chỉ có một bộ đếm duy nhất được duy trì ngoài bộ nhớ đầu vào | 

Các ràng buộc cho phép tối đa 2 × 10^5 phần tử và một lần quét tuyến tính duy nhất vừa vặn thoải mái trong cả giới hạn thời gian và bộ nhớ trong Python, vì nó chỉ thực hiện tăng số nguyên đơn giản cho mỗi phần tử. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    
    input = sys.stdin.readline
    n = int(input().strip())
    arr = list(map(int, input().split()))
    return str(sum(arr))

# provided samples
assert run("7\n1 0 1 1 0 0 1\n") == "4"
assert run("5\n0 1 0 0 1\n") == "2"

# minimum size
assert run("1\n0\n") == "0"
assert run("1\n1\n") == "1"

# all zeros
assert run("5\n0 0 0 0 0\n") == "0"

# all ones
assert run("6\n1 1 1 1 1 1\n") == "6"

# alternating pattern
assert run("8\n1 0 1 0 1 0 1 0\n") == "4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đơn 0 | 0 | trường hợp ranh giới tối thiểu | 
| đơn 1 | 1 | trường hợp hoạt động tối thiểu | 
| tất cả số không | 0 | không có đóng góp | 
| tất cả những cái | n | tích lũy tối đa | 
| xen kẽ | n/2 | độ chính xác phân phối hỗn hợp | 

## Vỏ cạnh 

Với n = 1 với giá trị 0, thuật toán khởi tạo bộ đếm về 0, đọc phần tử đơn, tìm thấy nó không phải là 1 và xuất ra 0. Điều này khớp trực tiếp với hành vi dự kiến vì không có đèn lồng sáng. 

Với n = 1 với giá trị 1, bộ đếm bắt đầu ở 0, tăng một lần và xuất ra 1. Điều này xác nhận tính chính xác ở kích thước đầu vào không tầm thường nhỏ nhất. 

Đối với trường hợp tất cả các số 0, chẳng hạn như n = 5 và [0, 0, 0, 0, 0], mỗi lần lặp lại khiến bộ đếm không thay đổi, do đó kết quả cuối cùng vẫn là 0, phù hợp với việc không có đèn lồng thắp sáng. 

Đối với tất cả các số 1, chẳng hạn như n = 5 và [1, 1, 1, 1, 1], mỗi lần lặp sẽ tăng bộ đếm, do đó nó tích lũy chính xác n. Điều này cho thấy thuật toán có tỷ lệ tuyến tính mà không cần bất kỳ xử lý đặc biệt nào đối với đầu vào dày đặc.
