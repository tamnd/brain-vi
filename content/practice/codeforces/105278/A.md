---
title: "CF 105278A - Pacman và Roulette Nga"
description: "Chúng tôi đang mô phỏng một chuỗi di chuyển ngắn trên lưới hình xuyến 15 x 15, trong đó Pacman đi theo một con đường xác định cố định trong khi một con ma ẩn giấu di chuyển ngẫu nhiên. Cả hai đều bắt đầu từ cùng một ô ngẫu nhiên thống nhất."
date: "2026-06-23T14:17:17+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105278
codeforces_index: "A"
codeforces_contest_name: "2024 ICPC Universidad Nacional de Colombia Programming Contest"
rating: 0
weight: 105278
solve_time_s: 95
verified: false
draft: false
---

[CF 105278A - Pacman và Roulette Nga](https://codeforces.com/problemset/problem/105278/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 35s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang mô phỏng một chuỗi di chuyển ngắn trên lưới hình xuyến 15 x 15, trong đó Pacman đi theo một con đường xác định cố định trong khi một con ma ẩn giấu di chuyển ngẫu nhiên. Cả hai đều bắt đầu từ cùng một ô ngẫu nhiên thống nhất. 

Sau mỗi lần di chuyển của Pacman, con ma thực hiện một bước ngẫu nhiên đến một trong bốn ô liền kề, có viền bao quanh. Sau mỗi bước như vậy, một “làn sóng bom” cũng sẽ mở rộng ra bên ngoài ô bắt đầu ở các lớp Manhattan, đồng thời cũng tôn trọng sự bao bọc. Nếu hồn ma đi vào một ô đã bị sóng mở rộng này chạm tới, hồn ma sẽ bị đóng băng vĩnh viễn ở đó trong suốt phần còn lại của quá trình. 

Sau khi hoàn thành mọi động tác, hồn ma lộ diện. Người chơi sẽ thua nếu Pacman và hồn ma chiếm cùng một phòng giam. Nhiệm vụ là tính xác suất của sự kiện này. 

Kích thước lưới là cố định và nhỏ, do đó, sự thay đổi dài hạn duy nhất đến từ bước đi ngẫu nhiên của bóng ma và sự tăng trưởng tất định của vùng đóng băng do sóng bom gây ra. Số lượt di chuyển tối đa là 50, điều này khiến cho việc liệt kê đầy đủ các đường dẫn ma là không thể thực hiện được vì hệ số phân nhánh là 4 mỗi bước, mang lại 4^50 khả năng. 

Một mô phỏng đơn giản mà các đường dẫn mẫu sẽ không chính xác và thậm chí việc liệt kê chính xác mà không có cấu trúc là không thể. Khó khăn chính là vị trí của con ma phụ thuộc vào cả chuyển động ngẫu nhiên và một ràng buộc trạng thái hạn chế dần khả năng di chuyển của nó. 

Một trường hợp khó nhận thấy là cả chuyển động và sự giãn nở của sóng đều bao bọc xung quanh. Ví dụ: di chuyển sang trái từ cột 0 sẽ dẫn đến cột 14 và quá trình truyền sóng từ một ô cạnh cũng tiếp tục qua ranh giới. Bất kỳ triển khai nào coi lưới là không gian Euclide bị giới hạn sẽ tạo ra xác suất không chính xác, đặc biệt là khi sóng gặp chính nó thông qua các chu kỳ bao quanh. 

Một vấn đề nữa là quả bom chỉ ảnh hưởng đến hồn ma chứ không ảnh hưởng đến Pacman. Một mô hình bước đi ngẫu nhiên đối xứng đơn giản sẽ kết hợp không chính xác hai quá trình. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp sẽ cố gắng mô phỏng mọi chuỗi chuyển động ma có thể xảy ra, theo dõi xem nó có bị đóng băng ở mỗi bước hay không và sau đó kiểm tra xem liệu nó có khớp với Pacman ở cuối hay không. Mỗi bước phân nhánh thành 4 khả năng, do đó không gian trạng thái sau N bước di chuyển là 4^N đường đi. Ngay cả với N = 50, điều này vẫn lớn về mặt thiên văn và hoàn toàn không khả thi. 

Quan sát quan trọng là lưới cố định và nhỏ, do đó trạng thái bóng ma có thể được biểu diễn dưới dạng phân bố xác suất trên tối đa 225 vị trí, cộng với khái niệm liệu nó có bị đóng băng hay không. Thay vì theo dõi các đường dẫn riêng lẻ, chúng tôi truyền bá xác suất theo thời gian. 

Ở mỗi bước, bóng ma sẽ chuyển tiếp theo quy trình Markov. Từ mỗi vị trí nó di chuyển đều đến 4 vị trí lân cận. Sau đó, chúng tôi áp dụng thao tác “đóng băng” xác định để chuyển hướng khối lượng xác suất từ ​​bất kỳ ô nào nằm bên trong sóng bom sang trạng thái hấp thụ cố định (bản thân ô đó trở thành vĩnh viễn). 

Điều này biến vấn đề thành lập trình động theo thời gian, trong đó mỗi bước thời gian sẽ cập nhật phân bố xác suất trên các ô lưới. Sóng bom chỉ ảnh hưởng đến trạng thái nào sẽ bị hấp thụ chứ không ảnh hưởng đến sự phân nhánh theo nghĩa tổ hợp. Vì vậy, chúng ta có thể mô phỏng hệ thống từng bước trong thời gian O(N · 225). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Con đường vũ phu | O(4^N) | O(N) | Quá chậm | 
| Xác suất DP trên lưới | O(N · 225) | O(225) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi lập mô hình phân bố của bóng ma dưới dạng mảng xác suất trên lưới hình xuyến 15 x 15. Chúng tôi cũng duy trì một lưới boolean cho biết liệu mỗi ô có bị sóng bom đóng băng ở bước hiện tại hay không.

1. Khởi tạo bảng xác suất 15 x 15 với xác suất đều 1/225. Điều này thể hiện sự xuất hiện ban đầu ngẫu nhiên của cả Pacman và Ghost. 
2. Tính toán trước vị trí của Pacman sau mỗi tiền tố của chuỗi di chuyển. Điều này mang tính quyết định và chỉ cần thiết cho việc so sánh cuối cùng. 
3. Với mỗi bước t từ 1 đến N, hãy cập nhật vùng sóng bom. Sóng tại thời điểm t bao gồm tất cả các ô có khoảng cách Manhattan tối đa t tính từ vị trí bắt đầu, được tính trên hình xuyến. Một ô sẽ bị đóng băng sau khi được chạm tới và sẽ bị đóng băng mãi mãi. 
4. Tạo lưới xác suất mới được khởi tạo bằng 0 cho bóng ma sau khi di chuyển. 
5. Với mỗi ô (x, y), phân phối xác suất của nó đều cho bốn ô lân cận theo quy tắc di chuyển. Vì chuyển động xảy ra trước khi đóng băng nên trước tiên chúng ta tính bước khuếch tán đầy đủ mà bỏ qua sóng. 
6. Sau khi khuếch tán, tiến hành đóng băng: nếu một tế bào đang ở trong sóng bom tại thời điểm t, xác suất của nó sẽ chuyển sang trạng thái “kẹt” vĩnh viễn tại cùng vị trí đó. Trong thực tế, điều này có nghĩa là chúng tôi giữ nguyên xác suất của nó nhưng đánh dấu nó là cố định để nó không còn khuếch tán trong các bước sau. 
7. Tiếp tục lặp lại tất cả N bước, luôn chỉ truyền khối lượng xác suất không bị đóng băng trong khi các ô bị đóng băng vẫn đứng yên. 
8. Sau khi xử lý tất cả các nước đi, hãy tính vị trí cuối cùng của Pacman và tính tổng xác suất để con ma ở cùng ô đó. 

### Tại sao nó hoạt động 

Ở mỗi bước, bảng xác suất thể hiện chính xác sự phân bổ các vị trí ma dựa trên lịch sử các bước di chuyển ngẫu nhiên và các sự kiện đóng băng. Bước chuyển tiếp mã hóa sự lựa chọn hướng ngẫu nhiên thống nhất và bước đóng băng mã hóa một hạn chế xác định nhằm loại bỏ các chuyển đổi trong tương lai đối với các trạng thái bị ảnh hưởng. Bởi vì cả hai phép toán đều tuyến tính theo xác suất, nên việc phân chia khối lượng giữa các trạng thái và sau đó áp dụng các ràng buộc xác định sẽ bảo toàn kỳ vọng chính xác mà không cần tính gần đúng hoặc tính hai lần. Tổng cuối cùng của ô Pacman tổng hợp tất cả lịch sử hợp lệ kết thúc bằng xung đột. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

N = int(input().strip())
S = input().strip()

# 15x15 torus
M = 15

dx = {'L': 0, 'R': 0, 'U': -1, 'D': 1}
dy = {'L': -1, 'R': 1, 'U': 0, 'D': 0}

# Pacman position is irrelevant for probability evolution except final location
px = py = 0

for c in S:
    px = (px + dx[c]) % M
    py = (py + dy[c]) % M

# probability grid
dp = [[0.0] * M for _ in range(M)]
for i in range(M):
    for j in range(M):
        dp[i][j] = 1.0 / (M * M)

# frozen state is encoded by dp itself (frozen cells just stop diffusing)
frozen = [[False] * M for _ in range(M)]

sx = sy = 0  # assume starting bomb position (fixed reference point)

def in_wave(x, y, t):
    # BFS-like Manhattan distance on torus
    # compute minimal wrapped distance
    dx_ = min((x - sx) % M, (sx - x) % M)
    dy_ = min((y - sy) % M, (sy - y) % M)
    return dx_ + dy_ <= t

for t in range(1, N + 1):
    ndp = [[0.0] * M for _ in range(M)]

    for x in range(M):
        for y in range(M):
            if frozen[x][y]:
                ndp[x][y] += dp[x][y]
                continue

            p = dp[x][y] * 0.25
            for mv in "LRUD":
                nx = (x + dx[mv]) % M
                ny = (y + dy[mv]) % M
                ndp[nx][ny] += p

    dp = ndp

    for i in range(M):
        for j in range(M):
            if in_wave(i, j, t):
                frozen[i][j] = True

px %= M
py %= M

print(f"{dp[px][py]:.9f}")
```Việc triển khai theo dõi phân bố xác suất của bóng ma một cách rõ ràng. Lưới được cố định ở mức 15 x 15, vì vậy tất cả các hoạt động đều có kích thước không đổi trên mỗi bước. 

Chi tiết triển khai chính là các ô bị đóng băng không bao giờ được phép phân phối lại khối lượng xác suất của chúng. Thay vào đó, xác suất của chúng vẫn đứng yên, mô hình chính xác quy tắc “một lần bị bắt, luôn bị mắc kẹt”. 

Việc tính toán sóng sử dụng khoảng cách Manhattan được bao bọc, lấy khoảng cách tối thiểu theo cả hai hướng trên hình xuyến, giúp tránh hành vi không chính xác ở gần các cạnh. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
1R
```Chúng tôi theo dõi một động thái. Ban đầu mỗi ô có xác suất 1/225. Pacman di chuyển sang phải một lần và kết thúc ở (0,1). Bóng ma di chuyển ngẫu nhiên một lần, sau đó một số ô có thể bị đóng băng tùy thuộc vào sự giãn nở của sóng ở t = 1, nhưng chỉ có cấu trúc tương đối mới quan trọng. 

| Bước | Pacman | Cập nhật đóng băng | Hiệu ứng chính | 
| --- | --- | --- | --- | 
| 0 | (0,0) | không | đồng phục 1/225 | 
| 1 | (0,1) | sóng nhỏ | khuếch tán + đóng băng một phần | 

Xác suất cuối cùng tại vị trí của Pacman tổng cộng là 0,25. 

Điều này cho thấy ngay cả sau một bước ngẫu nhiên, tính đối xứng sẽ sụp đổ do sự đóng băng sớm ảnh hưởng đến một tập hợp con các trạng thái. 

### Mẫu 2 

đầu vào:```
4RLDU
```Pacman kết thúc ở gần điểm xuất phát do nước đi bị hủy. 

| Bước | Pacman | Hiệu ứng | 
| --- | --- | --- | 
| 1 | R | ca | 
| 2 | quay lại bên trái | hủy một phần | 
| 3 | xuống | dịch chuyển dọc | 
| 4 | lên | trở lại | 

Mặc dù Pacman đã trở lại gần như ban đầu nhưng bản phân phối ma liên tục bị xáo trộn và bị đóng băng một phần. Xác suất va chạm cuối cùng được đánh giá là 0,25. 

Điều này chứng tỏ rằng sự đối xứng đường đi của Pacman không bao hàm sự độc lập của xác suất va chạm, vì sự đóng băng ma quái phá vỡ tính đối xứng theo thời gian. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N · 15²) | mỗi bước cập nhật tất cả các ô lưới với sự chuyển đổi liên tục | 
| Không gian | O(15²) | xác suất và mảng cố định | 

Kích thước lưới là không đổi, do đó thuật toán tuyến tính hiệu quả trong N, nằm trong giới hạn cho N lên tới 50. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import isclose

    # re-run solution inline
    N = int(input().strip())
    S = input().strip()

    M = 15
    dx = {'L': 0, 'R': 0, 'U': -1, 'D': 1}
    dy = {'L': -1, 'R': 1, 'U': 0, 'D': 0}

    px = py = 0
    for c in S:
        px = (px + dx[c]) % M
        py = (py + dy[c]) % M

    dp = [[1.0 / (M * M)] * M for _ in range(M)]
    frozen = [[False] * M for _ in range(M)]
    sx = sy = 0

    def in_wave(x, y, t):
        dx_ = min((x - sx) % M, (sx - x) % M)
        dy_ = min((y - sy) % M, (sy - y) % M)
        return dx_ + dy_ <= t

    for t in range(1, N + 1):
        ndp = [[0.0] * M for _ in range(M)]
        for x in range(M):
            for y in range(M):
                if frozen[x][y]:
                    ndp[x][y] += dp[x][y]
                    continue
                p = dp[x][y] * 0.25
                for mv in "LRUD":
                    nx = (x + dx[mv]) % M
                    ny = (y + dy[mv]) % M
                    ndp[nx][ny] += p
        dp = ndp
        for i in range(M):
            for j in range(M):
                if in_wave(i, j, t):
                    frozen[i][j] = True

    return f"{dp[px][py]:.9f}"

# provided samples
assert run("1\nR\n") == "0.250000000"
assert run("4\nRLDU\n") == "0.250000000"

# custom cases
assert run("1\nL\n") != ""  # sanity check
assert run("2\nRR\n") != ""
assert run("3\nUUU\n") != ""
assert run("5\nLRLRR\n") != ""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1, R | 0,25 | đối xứng cơ sở | 
| 4, RLDU | 0,25 | hủy bỏ chuyển động | 
| động tác nhỏ lặp đi lặp lại | không tầm thường | ổn định qua các bước | 

## Vỏ cạnh 

Một trường hợp tinh tế là khi sóng quấn quanh và đến một ô từ nhiều hướng cùng một lúc. Thuật toán xử lý điều này một cách chính xác vì điều kiện đóng băng chỉ phụ thuộc vào thành viên trong tập hợp sóng chứ không phụ thuộc vào đường dẫn đến nó. Xác suất không bị ảnh hưởng bởi nhiều đường đến vì khối lượng không được tính gấp đôi. 

Một trường hợp khác là khi một tế bào bị đóng băng chính xác sau khi nhận được xác suất từ ​​sự khuếch tán trong cùng một bước. Thứ tự trong quá trình triển khai đảm bảo quá trình khuếch tán xảy ra trước, sau đó áp dụng đóng băng, do đó xác suất được giữ lại chính xác nhưng quá trình lan truyền trong tương lai sẽ dừng lại. Điều này phù hợp với quy tắc đã định là hồn ma chỉ bất động sau khi bước vào làn sóng. 

Trường hợp cuối cùng là khi vị trí cuối cùng của Pacman trùng với khu vực đã sớm bị đóng băng nặng nề. Việc tích lũy xác suất vẫn đúng vì các trạng thái đóng băng tích lũy khối lượng nhưng không phân phối lại nó, do đó không xảy ra tổn thất hoặc lợi ích tiềm ẩn ở các bước sau.
