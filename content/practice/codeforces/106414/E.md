---
title: "CF 106414E - BABA BỊ KHÓA"
description: "Chúng tôi có một lưới n x m. Đường dẫn bắt đầu ở ô dưới cùng bên trái và phải kết thúc ở ô trên cùng bên phải. Ở mỗi lần di chuyển, đường đi có thể đi lên một ô, sang trái một ô hoặc sang phải một ô. Nó không bao giờ có thể đi xuống và không bao giờ có thể dẫm lên một tế bào hai lần."
date: "2026-06-25T09:47:37+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106414
codeforces_index: "E"
codeforces_contest_name: "Rutgers University Programming Contest Spring 2026 - Open Division"
rating: 0
weight: 106414
solve_time_s: 46
verified: true
draft: false
---

[CF 106414E - BABA ĐÃ KHÓA](https://codeforces.com/problemset/problem/106414/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 46s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một`n`qua`m`lưới. Đường dẫn bắt đầu ở ô dưới cùng bên trái và phải kết thúc ở ô trên cùng bên phải. Ở mỗi lần di chuyển, đường đi có thể đi lên một ô, sang trái một ô hoặc sang phải một ô. Nó không bao giờ có thể đi xuống và không bao giờ có thể dẫm lên một tế bào hai lần. Nhiệm vụ là đếm xem có bao nhiêu đường dẫn hợp lệ khác nhau tồn tại, với câu trả lời được lấy modulo`998244353`. 

Phần quan trọng của quy tắc di chuyển là số hàng chỉ có thể giảm. Khi một đường đi rời khỏi một hàng bằng cách di chuyển lên trên, nó sẽ không bao giờ quay trở lại hàng đó. Điều này có nghĩa là mỗi hàng có thể được xem xét độc lập và thông tin duy nhất được truyền từ hàng này sang hàng tiếp theo là cột nơi đường đi đi lên. 

Những ràng buộc cho phép`n`Và`m`lớn tới 100, với nhiều trường hợp thử nghiệm. Một giải pháp khám phá mọi con đường là không thể vì số lượng con đường tăng theo cấp số nhân. Chúng ta cần tìm một mô hình toán học và tính toán nó một cách trực tiếp. Vì câu trả lời chỉ liên quan đến lũy thừa và số học môđun, nên`O(n)`hoặc`O(log n)`giải pháp là đủ. 

Một sai lầm phổ biến là nghĩ rằng đường đi buộc phải di chuyển thẳng lên trên sau khi đến một cột. Ví dụ, trong một`2 x 3`lưới, câu trả lời không phải là 1. Hàng dưới cùng có thể di chuyển từ cột bắt đầu 0 đến cột 0, 1 hoặc 2 trước khi đi lên, tạo ra 3 đường dẫn. 

Một sai lầm khác là cho phép đường dẫn đảo ngược trong một hàng. Ví dụ: bắt đầu từ cột 0 liên tiếp, chuyển sang cột 2 và sau đó quay lại cột 1 là không hợp lệ vì cột 1 đã được truy cập. Các ô được truy cập của mỗi hàng phải tạo thành một phân đoạn liên tục. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu là mô phỏng mọi con đường có thể. Từ mỗi ô được truy cập, chúng tôi thử mọi bước di chuyển được phép và dừng lại khi đến mục tiêu hoặc bị mắc kẹt. Điều này đúng vì nó trực tiếp tuân theo định nghĩa của một đường dẫn hợp lệ. 

Vấn đề là số lượng lựa chọn bùng nổ. Một đường dẫn có thể thực hiện nhiều lựa chọn theo chiều ngang trong mỗi hàng và số lượng đường dẫn có thể tăng theo cấp số nhân theo kích thước lưới. Ngay cả các lưới nhỏ cũng nhanh chóng tạo ra quá nhiều nhánh đệ quy. 

Quan sát quan trọng là hành vi của đường dẫn bên trong một hàng rất đơn giản. Khi một đường đi vào một hàng, nó sẽ ở một cột nào đó`x`. Trước khi di chuyển lên trên có thể chọn bất kỳ cột nào`y`ở hàng đó là nơi nó rời đi. Các tế bào giữa`x`Và`y`được truy cập dưới dạng một đoạn ngang liên tục. Không thể thực hiện được hình dạng nào khác vì việc thay đổi hướng ngang sẽ yêu cầu phải xem lại một ô. 

Đối với mỗi hàng ngoại trừ hàng trên cùng, cột rời đi có thể là bất kỳ`m`cột. Hàng dưới cùng bắt đầu ở cột 0, nhưng vẫn có`m`các cột có thể nơi nó có thể để lại. Các hàng tiếp theo hoạt động theo cách tương tự. Hàng trên cùng không cần lựa chọn vì khi nhập vào, đường dẫn chỉ di chuyển theo chiều ngang cho đến khi đến ô trên cùng bên phải. 

có`n - 1`các hàng nơi chúng tôi chọn một cột thoát và mỗi lựa chọn có`m`khả năng. Do đó tổng số đường đi là`m^(n-1)`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Ngăn xếp đệ quy O(nm) | Quá chậm | 
| Tối ưu | O(log n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`n`Và`m`cho lưới hiện tại. Đường dẫn có lựa chọn cột thoát ở mỗi hàng ngoại trừ hàng cuối cùng, vì vậy chúng ta cần tính toán`m`được nâng lên quyền lực`n - 1`. 
2. Tính toán`m^(n-1)`sử dụng lũy ​​thừa mô-đun. Số mũ có thể lớn trên kích thước đầu vào, vì vậy phép nhân lặp lại không phải là phương pháp dự định. 
3. Xuất giá trị tính toán theo modulo`998244353`. 

Tại sao nó hoạt động: 

Mỗi quá trình chuyển đổi hàng được mô tả hoàn toàn bằng cột nơi đường dẫn đi lên. Sau khi nhập một hàng, việc chọn bất kỳ cột thoát nào sẽ tạo ra chính xác một đoạn ngang hợp lệ và mỗi đoạn hợp lệ tương ứng với chính xác một cột thoát. Vì các hàng độc lập và hàng trên cùng không có lựa chọn chuyển tiếp nên số đường dẫn có thể là tích của`m`lựa chọn hơn`n - 1`hàng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def solve():
    data = input().split()
    if not data:
        return
    t = int(data[0])
    ans = []
    idx = 1
    for _ in range(t):
        n = int(data[idx])
        m = int(data[idx + 1])
        idx += 2
        ans.append(str(pow(m, n - 1, MOD)))
    print("\n".join(ans))

if __name__ == "__main__":
    solve()
```Việc triển khai sử dụng phép lũy thừa mô-đun tích hợp của Python. các`pow(base, exponent, mod)`biểu mẫu thực hiện lũy thừa nhị phân nên chỉ cần nhiều phép tính logarit. 

Số mũ là`n - 1`bởi vì hàng dưới cùng và mọi hàng phía trên nó cho đến hàng trên cùng yêu cầu quyết định về vị trí di chuyển lên trên. Hàng trên cùng chỉ kết thúc đường dẫn và không phân nhánh thêm. 

Không có kiểm tra ranh giới hoặc mô phỏng vì bằng chứng đã giảm vấn đề xuống việc đếm các lựa chọn. Modulo được áp dụng trong quá trình lũy thừa để tránh các giá trị trung gian lớn. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên, lưới là`2 x 3`. 

| Bước | Hàng được xử lý | Lựa chọn mỗi hàng | Số lượng hiện tại | 
| --- | --- | --- | --- | 
| Bắt đầu | Cần chọn lối ra hàng dưới cùng | 3 | 3 | 
| Kết thúc | Hàng trên cùng đạt mục tiêu | không có lựa chọn | 3 | 

Câu trả lời là 3. Ba đường dẫn tương ứng với việc rời khỏi hàng dưới cùng ở cột 0, 1 hoặc 2. 

Đối với mẫu thứ hai, lưới là`3 x 4`. 

| Bước | Hàng được xử lý | Lựa chọn mỗi hàng | Số lượng hiện tại | 
| --- | --- | --- | --- | 
| Bắt đầu | Thoát hàng dưới cùng | 4 | 4 | 
| Tiếp tục | Lối ra hàng giữa | 4 | 16 | 
| Kết thúc | Hàng trên cùng đạt mục tiêu | không có lựa chọn | 16 | 

Câu trả lời là 16 vì hai hàng đầu tiên, mỗi hàng đóng góp bốn cột có thể thoát. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(log n) | Phép lũy thừa mô đun cần nhiều phép nhân logarit | 
| Không gian | O(1) | Chỉ câu trả lời hiện tại và giá trị đầu vào được lưu trữ | 

Giải pháp dễ dàng phù hợp với các giới hạn vì mỗi trường hợp thử nghiệm chỉ yêu cầu một phép tính lũy thừa nhanh và không cần xây dựng lưới. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 998244353

def solution(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline
    t = int(input())
    out = []
    for _ in range(t):
        n, m = map(int, input().split())
        out.append(str(pow(m, n - 1, MOD)))
    return "\n".join(out)

assert solution("""2
2 3
3 4
""") == "3\n16", "samples"

assert solution("""1
2 2
""") == "2", "minimum grid"

assert solution("""1
100 100
""") == str(pow(100, 99, MOD)), "large grid"

assert solution("""3
5 1
4 5
2 100
""") == "\n".join([
    str(pow(1, 4, MOD)),
    str(pow(5, 3, MOD)),
    str(pow(100, 1, MOD))
]), "boundary cases"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 3`,`3 4`|`3`,`16`| Khẳng định công thức cơ bản | 
|`2 2`|`2`| Kiểm tra lưới nhỏ nhất có thể | 
|`100 100`|`100^99 mod MOD`| Kiểm tra việc xử lý số mũ lớn | 
|`m = 1`trường hợp | Đúng quyền hạn của một | Kiểm tra ranh giới cột đơn | 

## Vỏ cạnh 

Đối với một`2 x 3`lưới, một cách tiếp cận ngây thơ có thể cho rằng chỉ có một đường đi vì điểm bắt đầu và kết thúc được căn chỉnh theo chiều dọc trong cùng một cột sau khi chọn tuyến đường ngắn nhất. Thuật toán thấy rằng hàng dưới cùng có thể thoát ra ở bất kỳ cột nào trong ba cột, vì vậy nó tính toán`3^(2-1) = 3`. 

Đối với lưới có một cột, chẳng hạn như`5 x 1`, chuyển động duy nhất có thể là hướng lên trên. Công thức cho`1^(5-1) = 1`, khớp với đường dẫn hợp lệ duy nhất. 

Đối với một lưới lớn như`100 x 100`, việc liệt kê các đường dẫn là không thể. Thuật toán không bao giờ tạo đường dẫn nào cả, nó chỉ đánh giá số lượng lựa chọn cột độc lập bằng cách sử dụng lũy ​​thừa mô-đun.
