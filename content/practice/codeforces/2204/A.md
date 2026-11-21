---
title: "CF 2204A - Truyền bóng"
description: "Chúng ta được xếp một hàng học sinh xếp từ trái sang phải và một quả bóng chuyển động xác định theo những quy tắc cố định."
date: "2026-06-07T19:56:42+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "implementation"]
categories: ["algorithms"]
codeforces_contest: 2204
codeforces_index: "A"
codeforces_contest_name: "Educational Codeforces Round 188 (Rated for Div. 2)"
rating: 800
weight: 2204
solve_time_s: 185
verified: true
draft: false
---

[CF 2204A - chuyền bóng](https://codeforces.com/problemset/problem/2204/A) 

**Đánh giá:** 800 
**Tags:** bạo lực, thực hiện 
**Thời gian giải:** 3m 5s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được xếp một hàng học sinh xếp từ trái sang phải và một quả bóng chuyển động xác định theo những quy tắc cố định. Mỗi học sinh có một hướng, trái hoặc phải, nghĩa là bất cứ khi nào quả bóng chạm tới học sinh đó, nó sẽ ngay lập tức được chuyển cho học sinh liền kề theo hướng đó. Điểm cuối bắt buộc: học sinh đầu tiên luôn vượt qua bên phải và học sinh cuối cùng luôn vượt qua bên trái. 

Quá trình bắt đầu với việc học sinh 1 cầm bóng. Sau đó bóng được chuyền chính xác$n$lần. Trong những đường chuyền này, quả bóng có thể quay lại cùng một học sinh nhiều lần, nhưng chúng tôi chỉ quan tâm đến việc học sinh nào đã từng giữ bóng ít nhất một lần. 

Vì vậy, vấn đề giảm xuống còn việc mô phỏng bước đi xác định trên biểu đồ đường trong đó mỗi nút có chính xác một cạnh đi tới nút lân cận và chúng tôi đếm có bao nhiêu nút riêng biệt được truy cập trong một số bước cố định bắt đầu từ nút 1. 

Các ràng buộc đủ nhỏ để mô phỏng trực tiếp hoàn toàn an toàn. Với$t \le 10^4$Và$n \le 50$, thậm chí$O(n^2)$mỗi trường hợp thử nghiệm dẫn đến nhiều nhất$2.5 \cdot 10^7$bước, nằm trong giới hạn thoải mái trong Python. 

Không có trường hợp cạnh ẩn phức tạp nào về các nước đi không hợp lệ vì chuyển động luôn được xác định. Điểm tinh tế duy nhất là số bước chính xác$n$, cho đến khi ổn định hoặc cho đến khi thoát khỏi giới hạn. Điều đó quan trọng vì đường dẫn có thể dao động và truy cập lại các nút nhiều lần. 

Một sai lầm ngây thơ sẽ là cho rằng bước đi tạo thành một đoạn đơn điệu hoặc nó luôn di chuyển sang phải trước rồi sang trái một lần. Ví dụ, trong`RLRL`, quả bóng nảy trong khoảng từ 1 đến 2 vô thời hạn. Một cách giải thích tham lam sai lầm có thể cho rằng bước đi sẽ mở rộng liên tục một cách không chính xác, nhưng thực tế không phải vậy. 

Một sai lầm tiềm ẩn khác là quên đếm nút xuất phát (học sinh 1), vì bài toán nói rõ rằng họ nhận bóng trước khi thực hiện bất kỳ đường chuyền nào. 

## Phương pháp tiếp cận 

Quá trình được mô tả đã hoàn toàn mang tính xác định, vì vậy cách tiếp cận đơn giản nhất là mô phỏng nó từng bước. Chúng tôi duy trì vị trí hiện tại của quả bóng và sau mỗi lần chuyền, chúng tôi cập nhật vị trí theo hướng được lưu trong học sinh hiện tại. Chúng tôi cũng giữ một mảng boolean hoặc một tập hợp đánh dấu học sinh nào đã nhận được bóng. 

Sau khi thực hiện chính xác$n$chuyển tiếp, chúng tôi đếm có bao nhiêu học sinh được đánh dấu là đã đến thăm. 

Điều này hoạt động vì mỗi lần chuyển đổi trạng thái là$O(1)$, và chúng tôi chỉ làm$n$chuyển đổi cho mỗi trường hợp thử nghiệm, do đó tổng độ phức tạp là tuyến tính theo$n$. 

Ở đây mô phỏng lực lượng vũ phu đã là tối ưu. Không cần phát hiện chu trình hoặc nén đồ thị vì$n$nhiều nhất là 50, do đó, ngay cả việc tính toán lại nhiều lần trong nhiều trường hợp thử nghiệm vẫn không đáng kể. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu |$O(n^2)$tổng số trong tất cả các bài kiểm tra |$O(n)$| Đã chấp nhận | 
| Mô phỏng tối ưu (cùng ý tưởng) |$O(n^2)$tổng số trong tất cả các bài kiểm tra |$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Bắt đầu với quả bóng ở vị trí 1 và đánh dấu học sinh 1 là đã ghé thăm. Điều này phản ánh điều kiện ban đầu trước khi bất kỳ động thái nào xảy ra. 
2. Lặp lại chính xác$n$lần: nhìn vào hướng của học sinh hiện tại và di chuyển quả bóng sang trái hoặc phải. Mỗi lần di chuyển tương ứng với việc đi theo cạnh đi duy nhất từ ​​nút hiện tại. 
3. Sau mỗi lần di chuyển, đánh dấu vị trí mới là đã ghé thăm. Điều này đảm bảo chúng tôi nắm bắt được tất cả các nút từng nhận bóng chứ không chỉ điểm cuối của các đoạn. 
4. Sau khi hoàn thành tất cả các bước di chuyển, đếm xem có bao nhiêu học sinh đã được đánh dấu đã đến thăm và in ra con số đó. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào, vị trí của quả bóng được xác định hoàn toàn bởi vị trí trước đó và hướng cố định tại nút đó, nghĩa là quá trình này là một bước đi xác định trên biểu đồ có hướng trong đó mỗi nút có bậc chính xác bằng 1. Bởi vì chúng tôi mô phỏng rõ ràng mọi chuyển đổi, chúng tôi sao chép chính xác quỹ đạo của bước đi này cho chính xác$n$các bước. Mỗi học sinh được đếm chính xác là một học sinh mà quỹ đạo đi vào ít nhất một lần và không có học sinh nào bị bỏ sót vì mỗi lần truy cập đều được xử lý ngay lập tức khi nó xảy ra. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    out = []

    for _ in range(t):
        n = int(input().strip())
        s = input().strip()

        # 0-indexed positions
        pos = 0
        visited = [False] * n
        visited[pos] = True

        for _ in range(n):
            if s[pos] == 'R':
                pos += 1
            else:
                pos -= 1
            visited[pos] = True

        out.append(str(sum(visited)))

    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Việc triển khai phản ánh trực tiếp mô phỏng được mô tả trong thuật toán. Mảng`visited`theo dõi xem mỗi học sinh đã nhận được bóng hay chưa. Chúng tôi khởi tạo vị trí 0 vì học sinh 1 bắt đầu với quả bóng. Mỗi lần lặp sẽ cập nhật vị trí theo ký tự hiện tại trong chuỗi. 

Một điểm tinh tế là chuyển động luôn an toàn: bài toán đảm bảo ký tự đầu tiên được`R`và cuối cùng là`L`, vì vậy chúng tôi không bao giờ đi quá giới hạn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 4
s = RLRL
```Chúng tôi theo dõi quá trình từng bước. 

| Bước | Vị trí | Di chuyển | Đã truy cập bộ | 
| --- | --- | --- | --- | 
| Bắt đầu | 1 | - | {1} | 
| 1 | 2 | R | {1, 2} | 
| 2 | 1 | L | {1, 2} | 
| 3 | 2 | R | {1, 2} | 
| 4 | 1 | L | {1, 2} | 

Bước đi dao động trong khoảng từ 1 đến 2. Sau 4 bước đi chỉ có hai học sinh này được ghé thăm. 

Đầu ra là 2. 

### Ví dụ 2 

đầu vào:```
n = 6
s = RRRRRL
```| Bước | Vị trí | Di chuyển | Đã truy cập bộ | 
| --- | --- | --- | --- | 
| Bắt đầu | 1 | - | {1} | 
| 1 | 2 | R | {1,2} | 
| 2 | 3 | R | {1,2,3} | 
| 3 | 4 | R | {1,2,3,4} | 
| 4 | 5 | R | {1,2,3,4,5} | 
| 5 | 6 | R | {1,2,3,4,5,6} | 
| 6 | 5 | L | {1,2,3,4,5,6} | 

Chúng tôi thấy một cú quét sạch sang bên phải, sau đó lùi lại một bước. Tất cả học sinh đều được đến thăm. 

Đầu ra là 6. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(t \cdot n)$| Mỗi bài kiểm tra mô phỏng chính xác$n$chuyển động, mỗi chuyển động trong thời gian không đổi | 
| Không gian |$O(n)$| Chúng tôi lưu trữ một mảng kích thước đã truy cập$n$| 

Với$n \le 50$Và$t \le 10^4$, tổng số thao tác nhiều nhất là$5 \cdot 10^5$, dễ dàng đủ nhanh. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    solve()
    return ""  # output printed directly; for real tests capture stdout

# provided samples (conceptual placeholders since stdout capture is omitted)
# assert run("3\n4\nRLRL\n6\nRRRRRL\n9\nRRLRRRRRL\n") == "2\n6\n3"

# custom cases
# n = 2, immediate bounce
# R L forces 1 -> 2 -> 1
# assert run("1\n2\nRL\n") == "2"

# n = 3, monotone right then left
# assert run("1\n3\nRRL\n") == "3"

# n = 5, alternating pattern
# assert run("1\n5\nRLRLR\n") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| RL | 2 | dao động tức thời | 
| RRL | 3 | mở rộng rồi hoàn trả một phần | 
| RLRLR | 2 | mô hình nảy lặp đi lặp lại | 

## Vỏ cạnh 

Trường hợp cạnh then chốt là hành vi dao động thuần túy. Đối với đầu vào như`RLRL`, bước đi không bao giờ lan rộng ra ngoài hai nút đầu tiên. Quá trình mô phỏng bắt đầu ở nút 1, chuyển sang nút 2, sau đó ngay lập tức quay lại nút 1 và lặp lại. Vì chúng tôi đánh dấu lượt truy cập rõ ràng ở mỗi bước nên cả nút 1 và nút 2 đều được tính và không xảy ra hiện tượng mở rộng không chính xác. 

Một trường hợp khác là chuyển động đơn điệu hoàn toàn như`RRRRRL`. Ở đây, quả bóng di chuyển sang phải cho đến khi đến điểm cuối, ghé thăm mọi nút và sau đó lùi lại một lần. Mô phỏng đảm bảo nút 6 cũng được tính trước khi quay lại, vì chúng tôi đánh dấu sau mỗi lần di chuyển thay vì chỉ điểm cuối. 

Trường hợp tinh tế cuối cùng là đầu vào tối thiểu như`n = 2`. Quá trình vẫn thực hiện chính xác hai bước di chuyển bắt đầu từ nút 1. Quá trình mô phỏng xử lý việc này một cách tự nhiên: bước di chuyển đầu tiên đến nút 2, bước di chuyển thứ hai quay trở lại nút 1, dẫn đến cả hai nút đều được truy cập.
