---
title: "CF 2033A - Sakurako và Kosuke"
description: "Chúng ta được yêu cầu xác định người chiến thắng trong trò chơi theo lượt trong đó hai người chơi di chuyển một dấu chấm dọc theo trục số bắt đầu từ vị trí 0. Sakurako luôn di chuyển sang trái bằng cách tăng số lẻ đơn vị trong lượt của mình: -1, -5, -9, v.v."
date: "2026-06-08T11:41:48+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 2033
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 981 (Div. 3)"
rating: 800
weight: 2033
solve_time_s: 103
verified: false
draft: false
---

[CF 2033A - Sakurako và Kosuke](https://codeforces.com/problemset/problem/2033/A) 

**Đánh giá:** 800 
**Tags:** thuật toán xây dựng, triển khai, toán học 
**Thời gian giải:** 1 phút 43s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu xác định người chiến thắng trong trò chơi theo lượt trong đó hai người chơi di chuyển một dấu chấm dọc theo trục số bắt đầu từ vị trí 0. Sakurako luôn di chuyển sang trái bằng cách tăng số lẻ đơn vị trong lượt của mình: -1, -5, -9, v.v. Kosuke di chuyển sang phải theo cùng một dãy số lẻ: 3, 7, 11, v.v. Trò chơi kết thúc ngay khi dấu chấm di chuyển ra ngoài khoảng từ`-n`ĐẾN`n`. Nhiệm vụ của chúng ta là xác định người chơi nào thực hiện nước đi hợp lệ cuối cùng trước khi dấu chấm thoát khỏi khoảng thời gian này. 

Đầu vào cung cấp nhiều trò chơi độc lập, mỗi trò chơi được xác định bởi một số nguyên duy nhất`n`. Đầu ra phải là tên của người chơi cuối cùng di chuyển trong mỗi trò chơi. Giới hạn là nhỏ:`t`lên tới 100 trò chơi và`n`lên đến 100. Điều này cho phép các thuật toán chạy ở mức O(n) tệ nhất cho mỗi trò chơi và thậm chí việc mô phỏng các bước di chuyển đơn giản cũng sẽ hoàn thành nhanh chóng. 

Các trường hợp cạnh phát sinh với các giá trị rất nhỏ của`n`. Ví dụ, nếu`n=1`, trình tự di chuyển là -1 (Sakurako), 3 (Kosuke). Sau hành động đầu tiên của Sakurako,`x=-1`đang ở trong khoảng thời gian nên trò chơi tiếp tục. Nước đi tiếp theo của Kosuke cộng thêm 3, lấy`x`đến 2, vượt quá`n=1`. Kosuke là người chơi cuối cùng thực hiện nước đi trong giới hạn nên anh ấy thắng. Một phép tính đơn giản hoặc quên dấu di chuyển sẽ tạo ra câu trả lời sai trong những trường hợp như vậy. 

## Phương pháp tiếp cận 

Cách tiếp cận đơn giản nhất là mô phỏng trò chơi cho từng`n`. Bắt đầu lúc`x=0`và lặp lại lần lượt, luân phiên người chơi. Lần lượt`i`, thêm vào`2*i-1`theo đúng hướng. Dừng lại khi bước tiếp theo sẽ diễn ra`x`ngoài`[-n, n]`. Điều này phù hợp với các ràng buộc nhất định vì mỗi bước di chuyển sẽ tăng về cường độ và`n`nhỏ nên số lần lặp trong mỗi trò chơi bị giới hạn gần bằng căn bậc hai của`2*n`(vì các số lẻ có tổng gần bằng bậc hai). Các hoạt động trong trường hợp xấu nhất trên mỗi trò chơi đều dưới 100, khiến điều này trở nên khả thi. 

Tuy nhiên, chúng ta có thể quan sát một mô hình. Nếu chúng ta tách các nước đi thành các bước âm của Sakurako và các bước dương của Kosuke thì tổng tích lũy của mỗi bên tạo thành một dãy số lẻ. Cho phép`S = sum_{k=1}^{m} (2k-1)`là tổng tích lũy của lần đầu tiên`m`di chuyển. Điều này luôn luôn`m^2`. Trò chơi về cơ bản tiếp tục cho đến khi tổng tích lũy bên trái vượt quá`n`hoặc tổng tích lũy bên phải vượt quá`n`. Do đó, thay vì mô phỏng mọi nước đi, chúng ta có thể tính toán trước bình tích lũy nào vượt quá đầu tiên.`n`cho mỗi bên. Người chơi có số tiền tích lũy vượt quá`n`tiếp theo thua và người chơi kia thực hiện nước đi hợp lệ cuối cùng. Điều này làm giảm vấn đề thành số học số nguyên đơn giản. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(sqrt(n)) mỗi trận | O(1) | Được chấp nhận với n ≤ 100 | 
| Phân tích tổng tích lũy tối ưu | O(1) mỗi trận | O(1) | Được chấp nhận và sạch sẽ | 

## Hướng dẫn thuật toán 

1. Đọc số trận đấu`t`. Khởi tạo một danh sách kết quả trống. 
2. Với mỗi trò chơi, hãy đọc`n`. Khởi tạo một biến vị trí`x = 0`và một bộ đếm chuyển động`i = 1`. 
3. Trong khi giá trị tuyệt đối của`x`ở trong`[-n, n]`, xác định lượng di chuyển hiện tại`2*i - 1`. Nếu như`i`là lẻ thì trừ đi (Sakurako di chuyển sang trái). Nếu như`i`chẵn, cộng nó (Kosuke di chuyển sang phải). Sau khi cập nhật`x`, tăng`i`. 
4. Khi nào`x`di chuyển đầu tiên ra ngoài khoảng`[-n, n]`, nước đi hợp lệ cuối cùng được thực hiện bởi người chơi trước đó. Nếu như`i`chẵn, Sakurako đã thực hiện nước đi hợp lệ cuối cùng. Nếu như`i`thật kỳ lạ, Kosuke đã thực hiện nước đi hợp lệ cuối cùng. 
5. Thêm tên người chiến thắng vào danh sách kết quả. 
6. Sau khi xử lý tất cả các trò chơi, hãy in kết quả. 

Tại sao nó hoạt động: các bước di chuyển tăng đều về cường độ và hướng thay thế. Bằng cách cập nhật`x`lần lượt, chúng tôi duy trì bất biến rằng`x`phản ánh đúng vị trí của dấu chấm. khoảnh khắc`x`vượt quá`[-n, n]`, chúng ta biết nước đi hợp lệ cuối cùng là nước đi trước đó, đảm bảo tính đúng đắn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
results = []

for _ in range(t):
    n = int(input())
    x = 0
    i = 1
    while True:
        move = 2*i - 1
        if i % 2 == 1:
            x -= move
        else:
            x += move
        if abs(x) > n:
            winner = "Sakurako" if i % 2 == 1 else "Kosuke"
            results.append(winner)
            break
        i += 1

print("\n".join(results))
```Mã bắt đầu bằng cách đọc số lượng trò chơi. Đối với mỗi trò chơi, nó khởi tạo dấu chấm ở mức 0 và mô phỏng từng bước di chuyển. Độ lớn chuyển động là`2*i-1`. Sakurako di chuyển sang trái ở lượt lẻ, Kosuke di chuyển sang phải ở lượt chẵn. Khi dấu chấm ra khỏi giới hạn, chúng tôi kiểm tra xem đến lượt ai và ghi lại người chiến thắng. Cách tiếp cận này xử lý cẩn thận các dấu hiệu và tính chẵn lẻ ở lượt, đây là những nguyên nhân dễ dẫn đến sai sót riêng lẻ. 

## Ví dụ đã hoạt động 

Hãy xem xét`n=6`. Các bước di chuyển là: -1 (Sakurako), +3 (Kosuke), -5 (Sakurako), +7 (Kosuke). Theo dõi`x`: 

| Di chuyển tôi | Người chơi | Di chuyển giá trị | x sau khi di chuyển | Trong giới hạn? | 
| --- | --- | --- | --- | --- | 
| 1 | Sakurako | -1 | -1 | vâng | 
| 2 | Kosuke | +3 | 2 | vâng | 
| 3 | Sakurako | -5 | -3 | vâng | 
| 4 | Kosuke | +7 | 4 | vâng | 
| 5 | Sakurako | -9 | -5 | vâng | 
| 6 | Kosuke | +11 | 6 | vâng | 
| 7 | Sakurako | -13 | -7 | không | 

Nước đi thứ 7 của Sakurako sẽ được thực hiện`x`đến -7, vượt quá`n=6`. Kosuke đã thực hiện nước đi hợp lệ cuối cùng tại`x=6`. Vậy người chiến thắng là Sakurako. 

Vì`n=3`: 

| Di chuyển tôi | Người chơi | Di chuyển giá trị | x sau khi di chuyển | Trong giới hạn? | 
| --- | --- | --- | --- | --- | 
| 1 | Sakurako | -1 | -1 | vâng | 
| 2 | Kosuke | +3 | 2 | vâng | 
| 3 | Sakurako | -5 | -3 | vâng | 
| 4 | Kosuke | +7 | 4 | không | 

Nước đi thứ 4 của Kosuke vượt quá giới hạn nên Sakurako đã thực hiện nước đi hợp lệ cuối cùng. Đầu ra là "Kosuke". 

Những dấu vết này xác nhận rằng việc kiểm tra vị trí từng nước đi và đánh giá tính chẵn lẻ của lượt sẽ xác định chính xác người chiến thắng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(sqrt(n) * t) | Số nước đi của mỗi ván đấu tăng theo số lẻ, tổng tăng theo phương trình bậc hai, nên khoảng √n nước đi mỗi ván. Với t ≤ 100, điều này là nhanh. | 
| Không gian | O(t) | Chúng tôi lưu trữ một chuỗi cho mỗi trò chơi để in ở cuối. Tất cả các biến khác là không đổi. | 

Với các ràng buộc, mô phỏng này thực hiện dưới 1 mili giây cho mỗi trường hợp thử nghiệm, nằm trong giới hạn 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    t = int(input())
    results = []
    for _ in range(t):
        n = int(input())
        x = 0
        i = 1
        while True:
            move = 2*i - 1
            if i % 2 == 1:
                x -= move
            else:
                x += move
            if abs(x) > n:
                winner = "Sakurako" if i % 2 == 1 else "Kosuke"
                results.append(winner)
                break
            i += 1
    return "\n".join(results)

# Provided samples
assert run("4\n1\n6\n3\n98\n") == "Kosuke\nSakurako\nKosuke\nSakurako", "sample 1"

# Custom test cases
assert run("1\n1\n") == "Kosuke", "minimum n"
assert run("1\n100\n") == "Sakurako", "maximum n"
assert run("2\n2\n5\n") == "Kosuke\nSakurako", "small odd-even n"
assert run("1\n10\n") == "Sakurako", "general medium n"
assert run("1\n50\n") == "Sakurako", "larger n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | Kosuke | Trường hợp n cạnh tối thiểu, đảm bảo lượt đầu tiên được xử lý chính xác | 
| 100 | Sakurako | N tối đa, đảm bảo mô phỏng xử lý các số lớn hơn | 
| 2,5 | Kosuke, Sakurako | Kiểm tra luân phiên và trình tự nhỏ | 
| 10 | Sak | |
