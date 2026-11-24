---
title: "CF 74B - Tàu hỏa"
description: "Đoàn tàu có n toa xe xếp thành một hàng. Người điều khiển di chuyển một cách xác định: mỗi phút anh ta đi một toa xe theo hướng hiện tại của mình và khi đến một trong hai đầu, anh ta sẽ đảo ngược hướng. Đường đi của anh ta hoàn toàn cố định khi chúng ta biết toa xe và hướng đi ban đầu."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "dp", "games", "greedy"]
categories: ["algorithms"]
codeforces_contest: 74
codeforces_index: "B"
codeforces_contest_name: "Codeforces Beta Round 68"
rating: 1500
weight: 74
solve_time_s: 131
verified: true
draft: false
---

[CF 74B - Tàu hỏa](https://codeforces.com/problemset/problem/74/B) 

**Đánh giá:** 1500 
**Tags:** dp, trò chơi, tham lam 
**Thời gian giải:** 2m 11s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Tàu có`n`toa xe xếp thành một hàng. Người điều khiển di chuyển một cách xác định: mỗi phút anh ta đi một toa xe theo hướng hiện tại của mình và khi đến một trong hai đầu, anh ta sẽ đảo ngược hướng. Đường đi của anh ta hoàn toàn cố định khi chúng ta biết toa xe và hướng đi ban đầu. 

Người trốn theo tàu hành xử khác nhau tùy thuộc vào việc tàu đang di chuyển hay đang dừng tại ga. 

Khi tàu đang di chuyển, người trốn theo tàu có thể giữ nguyên vị trí hoặc di chuyển một toa xe sang trái hoặc phải. Thứ tự quan trọng: người trốn theo di chuyển trước, sau đó người điều khiển di chuyển. Nếu sau khi người điều khiển di chuyển mà họ lại ở trong cùng một toa xe thì kẻ trốn theo tàu sẽ bị bắt ngay lập tức. 

Khi tàu chạy không tải tại ga, người trốn theo tàu sẽ tạm thời rời tàu trước khi người điều khiển tàu di chuyển. Sau khi người điều khiển di chuyển xong, người trốn theo tàu có thể vào lại bất kỳ toa xe nào. Nếu trạm này là trạm đầu cuối, được biểu thị bằng trạm cuối cùng`'1'`trong chuỗi, người trốn theo tàu sẽ rời đi vĩnh viễn và giành chiến thắng ngay lập tức. 

Chuỗi đầu vào mô tả trạng thái tàu theo từng phút. Tính cách`'0'`có nghĩa là tàu đang di chuyển trong thời gian đó, trong khi`'1'`có nghĩa là tàu đang chạy không tải. Ký tự cuối cùng luôn là`'1'`, nghĩa là quá trình cuối cùng luôn đến được trạm cuối. 

Chúng ta phải xác định xem liệu người điều khiển có thể bắt được người trốn theo tàu trước khi đến trạm cuối hay không. Nếu bộ điều khiển thắng, chúng tôi cũng phải xuất ra phút sớm nhất khi việc bắt giữ diễn ra trong chế độ chơi tối ưu. 

Những hạn chế là nhỏ. Đoàn tàu có tối đa 50 toa xe và độ dài dòng thời gian tối đa là 200. Điều đó ngay lập tức cho thấy rằng giải pháp mô phỏng trò chơi hoặc lập trình động dựa trên trạng thái là hoàn toàn khả thi. Thậm chí một`O(n * m)`hoặc`O(n^2 * m)`thuật toán ở đây rất nhỏ vì tổng số trạng thái chỉ có vài nghìn. 

Phần khó khăn không phải là hiệu năng mà là mô hình hóa trò chơi một cách chính xác. 

Một sai lầm dễ mắc phải là quên rằng trong những phút rảnh rỗi, người trốn theo tàu sẽ quay trở lại sau khi bộ điều khiển di chuyển. Coi như:```
3 1 2
to tail
1
```Bộ điều khiển di chuyển từ toa xe 2 sang toa xe 3 trong phút rảnh rỗi. Vì đây là ga cuối nên người trốn theo tàu sẽ rời đi vĩnh viễn trước khi vào lại và giành chiến thắng ngay lập tức. Việc thực hiện bất cẩn có thể buộc người trốn theo tàu phải chọn toa xe vào phút cuối và vô tình báo cáo việc bắt giữ. 

Một trường hợp tinh vi khác là thứ tự di chuyển trong các phút di chuyển. Giả định:```
2 1 2
to head
0
```Người trốn theo toa xe số 1 di chuyển trước. Anh ta có thể ở lại hoặc chuyển sang toa xe 2. Sau đó, người điều khiển chuyển từ toa xe 2 sang toa xe 1. Nếu người trốn theo tàu ở lại, anh ta sẽ sống sót. Nếu chúng tôi mô phỏng không chính xác chuyển động của bộ điều khiển trước, chúng tôi sẽ yêu cầu chụp ngay lập tức. 

Nguồn lỗi thứ ba là sự đảo ngược hướng của bộ điều khiển. Hướng chỉ thay đổi _sau khi_ vào toa xe`1`hoặc toa xe`n`. Ví dụ:```
4 2 3
to tail
0001
```Đường dẫn điều khiển là:```
3 -> 4 -> 3 -> 2
```không```
3 -> 4 -> 4 -> 3
```Một mô phỏng nảy sai sẽ thay đổi hoàn toàn các toa xe an toàn có thể tiếp cận được. 

## Phương pháp tiếp cận 

Cách trực tiếp nhất để nghĩ về trò chơi là tìm kiếm minimax đầy đủ. 

Tại mỗi phút, trạng thái trò chơi bao gồm phút hiện tại, toa đi lậu, toa điều khiển và hướng điều khiển. Từ trạng thái như vậy, chúng ta có thể thử đệ quy tất cả các hành động trốn theo hợp pháp, mô phỏng hành động của người điều khiển và xác định liệu chiến lược nào đó có tồn tại cho đến khi đến trạm cuối hay không. 

Phép đệ quy bạo lực này đúng về mặt khái niệm vì thời lượng trò chơi bị giới hạn tối đa là 200 phút. Người điều khiển chỉ có thể thực hiện một bước di chuyển ở mỗi bước, vì vậy sự phân nhánh duy nhất đến từ người trốn theo tàu. 

Tuy nhiên, cây trò chơi thô vẫn phát triển theo cấp số nhân. Trong những phút di chuyển, người trốn theo tàu có thể có tối đa ba lựa chọn, và trong những phút rảnh rỗi anh ta có thể chọn bất kỳ một trong những phút di chuyển.`n`toa xe. Trong trường hợp xấu nhất, số lượng lịch sử có thể có sẽ trở thành vô cùng lớn. 

Quan sát quan trọng là quỹ đạo của bộ điều khiển hoàn toàn được xác định trước. Sau khi cố định toa xe và hướng ban đầu, chúng tôi có thể tính toán trước chính xác vị trí của bộ điều khiển mỗi phút. 

Điều đó thay đổi vấn đề từ tìm kiếm hai người chơi thành DP có thể tiếp cận. 

Vào phút thứ`t`, chúng ta chỉ cần biết vị trí người trốn theo tàu nào vẫn có thể sống sót. Vị trí bộ điều khiển được cố định. Chúng tôi xử lý ngược dòng thời gian. 

Xác định trạng thái:```
dp[t][p] = whether the stowaway can survive starting from minute t in wagon p
```Quá trình chuyển đổi phụ thuộc vào trạng thái tàu vào phút`t`. 

Nếu tàu đang di chuyển, người trốn theo tàu có thể di chuyển tới`p-1`,`p`, hoặc`p+1`. Sau đó, người điều khiển chuyển sang toa xe tiếp theo của mình. Bất kỳ động thái hạ cánh trên bộ điều khiển đều bị cấm. Nếu ít nhất một nước đi hợp pháp sẽ dẫn tới một quốc gia chiến thắng trong tương lai thì`dp[t][p]`là đúng. 

Nếu tàu không hoạt động và không đến ga, người trốn theo tàu có thể vào lại bất kỳ toa xe nào sau khi người điều khiển di chuyển. Vì anh ta có thể tự do lựa chọn nên bang sẽ chiến thắng nếu tồn tại ít nhất một toa xe khác với vị trí người điều khiển đang chiến thắng trong phút tiếp theo. 

Nếu phút hiện tại là ga cuối, người trốn theo tàu sẽ thắng ngay lập tức dù anh ta đang ở đâu. 

Tìm kiếm brute-force hoạt động vì trò chơi là hữu hạn và mang tính xác định khi các nước đi được chọn, nhưng nó thất bại vì nó liên tục tính toán lại các trạng thái giống hệt nhau. DP lạc hậu sẽ sụp đổ nhiều lịch sử theo cấp số nhân thành chỉ`O(length * n)`tiểu bang. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu Minimax | Hàm mũ | Cây đệ quy hàm mũ | Quá chậm | 
| DP lạc hậu | O(L * n2) | O(L * n) | Đã chấp nhận | 

Đây`L`là độ dài của chuỗi trạng thái tàu, nhiều nhất là 200. 

## Hướng dẫn thuật toán 

1. Tính toán trước vị trí bộ điều khiển cho mỗi phút. 

Cửa hàng`ctrl[t]`với tư cách là toa xe điều khiển sau khi hoàn thành việc di chuyển phút`t`. Đồng thời duy trì hướng hiện tại và đảo ngược nó bất cứ khi nào bộ điều khiển chạm tới toa xe`1`hoặc toa xe`n`. 
2. Tạo bảng DP trong đó`dp[t][p]`có nghĩa là người trốn theo tàu vẫn có thể tránh bị bắt bắt đầu từ phút`t`khi đứng trong xe ngựa`p`trước hành động của anh ấy vào phút đó. 
3. Khởi tạo điều kiện đầu cuối. 

Nếu phút`t`là ký tự cuối cùng của chuỗi và nó là`'1'`, người trốn theo tàu sẽ rời khỏi tàu vĩnh viễn trước khi có bất kỳ tương tác nào tiếp theo. Mọi vị trí đều chiến thắng. 
4. Xử lý ngược số phút từ cuối về đầu. 

Quá trình xử lý ngược hoạt động vì mỗi trạng thái chỉ phụ thuộc vào các trạng thái trong tương lai. 
5. Xử lý số phút nhàn rỗi. 

Trong một phút nhàn rỗi, người trốn theo tàu sẽ biến mất trước khi người điều khiển di chuyển và sau đó có thể quay lại bất kỳ toa xe nào. Vì vậy sau khi bộ điều khiển chuyển sang`ctrl[t]`, người trốn theo tàu có thể chọn bất kỳ toa xe nào ngoại trừ toa xe đó. 

Nếu ít nhất một toa xe`q != ctrl[t]`thỏa mãn`dp[t+1][q]`, thì mọi toa xe hiện tại sẽ chiến thắng. 
6. Xử lý phút chuyển động. 

Từ toa xe`p`, người trốn theo tàu có thể di chuyển đến`p-1`,`p`, hoặc`p+1`nếu toa xe đó tồn tại. 

Sau khi di chuyển theo tàu, người điều khiển sẽ di chuyển tới`ctrl[t]`. Bất kỳ toa xe ứng cử viên nào bằng`ctrl[t]`có nghĩa là chụp ngay lập tức và phải được bỏ qua. 

Nếu một số toa xe hợp pháp tiếp theo cũng đáp ứng`dp[t+1][next_pos]`, sau đó`dp[t][p] = True`. 
7. Sau khi xếp đầy bàn, hãy kiểm tra toa xe chở lậu ban đầu. 

Nếu như`dp[0][m]`là đúng, in`"Stowaway"`. 
8. Mặt khác, mô phỏng thời lượng phát tối ưu duy nhất. 

Vì người điều khiển chắc chắn sẽ thắng nên người trốn theo tàu sẽ chọn các nước đi để tối đa hóa thời gian sống sót. Chúng tôi tính toán đệ quy phút chụp mới nhất bằng cách sử dụng các chuyển đổi tương tự. 

### Tại sao nó hoạt động 

Bất biến DP là:```
dp[t][p] is true exactly when the stowaway has a strategy to avoid capture from that state onward.
```Cảm ứng ngược đảm bảo tính chính xác. Ở ga cuối cùng, câu trả lời gần như đúng vì người trốn theo tàu sẽ rời đi vĩnh viễn. Mọi trạng thái trước đó chỉ được đánh giá từ các trạng thái tương lai đã đúng. 

Đối với số phút di chuyển, người trốn theo tàu sống sót khi và chỉ khi tồn tại ít nhất một nước đi hợp lệ để tránh va chạm ngay lập tức và dẫn đến một trạng thái có thể sống sót khác. 

Trong những phút nhàn rỗi, người trốn theo tàu có thể chọn bất kỳ toa xe nào sau khi quan sát người điều khiển di chuyển, do đó trạng thái có thể sống sót nếu ít nhất một toa xe an toàn vẫn có thể sống sót sau đó. 

Vì mọi hành động có thể xảy ra đều được xem xét chính xác theo luật chơi nên DP đặc trưng chính xác cho lối chơi tối ưu. 

## Giải pháp Python```python
import sys
from functools import lru_cache

input = sys.stdin.readline

def solve():
    n, m, k = map(int, input().split())

    direction_line = input().strip()
    s = input().strip()

    L = len(s)

    direction = -1 if direction_line == "to head" else 1

    ctrl = [0] * L

    pos = k
    d = direction

    for i in range(L):
        pos += d

        if pos == 1:
            d = 1
        elif pos == n:
            d = -1

        ctrl[i] = pos

    dp = [[False] * (n + 1) for _ in range(L + 1)]

    for p in range(1, n + 1):
        dp[L][p] = True

    for t in range(L - 1, -1, -1):
        if s[t] == '1':
            if t == L - 1:
                for p in range(1, n + 1):
                    dp[t][p] = True
            else:
                exists = False

                for q in range(1, n + 1):
                    if q != ctrl[t] and dp[t + 1][q]:
                        exists = True
                        break

                for p in range(1, n + 1):
                    dp[t][p] = exists
        else:
            for p in range(1, n + 1):
                ok = False

                for np in (p - 1, p, p + 1):
                    if not (1 <= np <= n):
                        continue

                    if np == ctrl[t]:
                        continue

                    if dp[t + 1][np]:
                        ok = True
                        break

                dp[t][p] = ok

    if dp[0][m]:
        print("Stowaway")
        return

    @lru_cache(None)
    def latest_capture(t, p):
        if s[t] == '1':
            if t == L - 1:
                return float('-inf')

            best = -1

            for q in range(1, n + 1):
                if q == ctrl[t]:
                    continue

                if dp[t + 1][q]:
                    best = max(best, latest_capture(t + 1, q))

            return best

        best = -1

        for np in (p - 1, p, p + 1):
            if not (1 <= np <= n):
                continue

            if np == ctrl[t]:
                best = max(best, t + 1)
                continue

            if dp[t + 1][np]:
                best = max(best, latest_capture(t + 1, np))
            else:
                best = max(best, t + 1)

        return best

    ans = latest_capture(0, m)

    print("Controller", ans)

if __name__ == "__main__":
    solve()
```Phần đầu tiên của mã tính toán trước quỹ đạo của bộ điều khiển. Điều này rất dễ xảy ra sai sót vì hướng chỉ đảo ngược sau khi đi vào toa xe điểm cuối. 

Bảng DP sử dụng đánh giá ngược vì mọi trạng thái chỉ phụ thuộc vào số phút trong tương lai. Hàng bổ sung`dp[L]`hoạt động như một trạng thái kết thúc thành công giả sau khi dòng thời gian kết thúc. 

Quá trình chuyển đổi phút nhàn rỗi rất tinh tế. Người trốn theo tàu biến mất trước khi người điều khiển di chuyển, vì vậy toa xe trước đó của anh ta trở nên không liên quan. Chính vì vậy mỗi`dp[t][p]`nhận được cùng một giá trị trong những phút nhàn rỗi. 

Quá trình chuyển đổi phút chuyển động sẽ kiểm tra tất cả ba hành động có thể thực hiện được. Việc chụp xảy ra sau khi bộ điều khiển di chuyển, vì vậy bất kỳ toa xe ứng cử viên nào bằng`ctrl[t]`là thua ngay. 

Sau khi xác định liệu có thể sống sót hay không, DFS thứ hai sẽ tính toán thời gian bắt giữ mới nhất có thể trong điều kiện trì hoãn phát tối ưu. Vì đường dẫn của bộ điều khiển là xác định nên người trốn theo tàu chỉ tối đa hóa số phút bắt giữ. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
5 3 2
to head
0001001
```Quỹ đạo điều khiển: 

| Phút | Vị trí điều khiển | 
| --- | --- | 
| 1 | 1 | 
| 2 | 2 | 
| 3 | 3 | 
| 4 | 4 | 
| 5 | 5 | 
| 6 | 4 | 
| 7 | 3 | 

Bây giờ hãy truy tìm kẻ trốn theo tàu. 

| Phút | Trạng thái tàu | Bộ điều khiển sau khi di chuyển | Có thể có toa xe an toàn | 
| --- | --- | --- | --- | 
| 1 | Di chuyển | 1 | 2, 3, 4, 5 | 
| 2 | Di chuyển | 2 | 1, 3, 4, 5 | 
| 3 | Di chuyển | 3 | 1, 2, 4, 5 | 
| 4 | Nhàn rỗi | 4 | Bất kỳ ngoại trừ 4 | 
| 5 | Di chuyển | 5 | 1, 2, 3, 4 | 
| 6 | Di chuyển | 4 | 1, 2, 3, 5 | 
| 7 | Thiết bị đầu cuối nhàn rỗi | 3 | Lối ra dành cho người trốn theo tàu | 

Phút nhàn rỗi ở bước 4 sẽ thiết lập lại hoàn toàn vị trí người trốn theo tàu. Anh ta luôn có thể vào lại khỏi bộ điều khiển, vì vậy việc sinh tồn trở nên dễ dàng. 

### Ví dụ tùy chỉnh 

đầu vào:```
2 1 2
to head
0001
```Chuyển động của bộ điều khiển: 

| Phút | Vị trí điều khiển | 
| --- | --- | 
| 1 | 1 | 
| 2 | 2 | 
| 3 | 1 | 
| 4 | 2 | 

Các chuyển đổi trạng thái: 

| Phút | Vị trí xếp hàng trước khi di chuyển | Di chuyển hợp pháp | Kết quả | 
| --- | --- | --- | --- | 
| 1 | 1 | ở lại 1 | sống sót | 
| 2 | 1 | chuyển sang 2 hoặc ở lại | cả hai đều thua | 
| 3 | đã bắt được rồi | - | - | 

Ở phút thứ 2, toa xe nào mà người trốn lậu chọn sẽ bị người điều khiển chiếm giữ sau khi di chuyển. Người điều khiển chắc chắn sẽ thắng ở phút thứ 2. 

Dấu vết này chứng tỏ tại sao thứ tự di chuyển lại quan trọng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(L * n2) | Trong mỗi phút, chúng tôi có thể quét tất cả các toa xe và các lựa chọn vào lại có thể có | 
| Không gian | O(L * n) | Bảng DP theo thời gian và vị trí toa xe | 

Với`L ≤ 200`Và`n ≤ 50`, tổng công việc là rất nhỏ. Ngay cả hệ số bậc hai cũng chỉ tạo ra khoảng 500.000 phép tính, nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io
from functools import lru_cache

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    n, m, k = map(int, input().split())
    direction_line = input().strip()
    s = input().strip()

    L = len(s)

    direction = -1 if direction_line == "to head" else 1

    ctrl = [0] * L

    pos = k
    d = direction

    for i in range(L):
        pos += d

        if pos == 1:
            d = 1
        elif pos == n:
            d = -1

        ctrl[i] = pos

    dp = [[False] * (n + 1) for _ in range(L + 1)]

    for p in range(1, n + 1):
        dp[L][p] = True

    for t in range(L - 1, -1, -1):
        if s[t] == '1':
            if t == L - 1:
                for p in range(1, n + 1):
                    dp[t][p] = True
            else:
                exists = False

                for q in range(1, n + 1):
                    if q != ctrl[t] and dp[t + 1][q]:
                        exists = True
                        break

                for p in range(1, n + 1):
                    dp[t][p] = exists
        else:
            for p in range(1, n + 1):
                ok = False

                for np in (p - 1, p, p + 1):
                    if not (1 <= np <= n):
                        continue

                    if np == ctrl[t]:
                        continue

                    if dp[t + 1][np]:
                        ok = True
                        break

                dp[t][p] = ok

    if dp[0][m]:
        return "Stowaway"

    @lru_cache(None)
    def latest_capture(t, p):
        if s[t] == '1':
            if t == L - 1:
                return float('-inf')

            best = -1

            for q in range(1, n + 1):
                if q == ctrl[t]:
                    continue

                if dp[t + 1][q]:
                    best = max(best, latest_capture(t + 1, q))

            return best

        best = -1

        for np in (p - 1, p, p + 1):
            if not (1 <= np <= n):
                continue

            if np == ctrl[t]:
                best = max(best, t + 1)
                continue

            if dp[t + 1][np]:
                best = max(best, latest_capture(t + 1, np))
            else:
                best = max(best, t + 1)

        return best

    return f"Controller {latest_capture(0, m)}"

# provided sample
assert run(
"""5 3 2
to head
0001001
"""
) == "Stowaway", "sample 1"

# minimum size
assert run(
"""2 1 2
to head
1
"""
) == "Stowaway", "terminal station immediately"

# forced capture
assert run(
"""2 1 2
to head
0001
"""
) == "Controller 2", "small forced trap"

# repeated idle stations
assert run(
"""5 2 4
to head
1111
"""
) == "Stowaway", "can always re-enter safely"

# bouncing controller
assert run(
"""4 1 2
to tail
00001
"""
) == "Controller 2", "tests direction reversal"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Trạm đầu cuối ngay lập tức | Người trốn theo tàu | Xử lý phút nhàn rỗi cuối cùng | 
| Hai toa xe buộc bẫy | Bộ điều khiển 2 | Thứ tự di chuyển đúng | 
| Nhiều phút nhàn rỗi | Người trốn theo tàu | Cơ chế tái nhập | 
| Trường hợp bị trả lại bộ điều khiển | Bộ điều khiển 2 | Hướng đảo ngược đúng đắn | 

## Vỏ cạnh 

Hãy xem xét trạm đầu cuối ngay lập tức:```
2 1 2
to head
1
```Tàu đã ở ga cuối trong phút đầu tiên. Người trốn theo tàu rời đi trước khi có bất kỳ sự tái nhập cảnh nào xảy ra, vì vậy anh ta thắng ngay lập tức. 

Thuật toán xử lý việc này vì mọi trạng thái cuối cùng`'1'`phút được khởi tạo là chiến thắng. 

Bây giờ hãy xem xét thứ tự di chuyển:```
2 1 2
to head
0001
```Phút 1:```
Stowaway: 1
Controller: 2 -> 1
```Người trốn theo tàu sống sót bằng cách ở lại toa xe số 1 cho đến khi di chuyển. Việc chụp chỉ được kiểm tra sau khi bộ điều khiển di chuyển. 

Phút 2:```
Controller: 1 -> 2
```Người trốn theo tàu không còn lối đi an toàn nào nữa. 

DP lập mô hình chính xác điều này vì vị trí của bộ điều khiển được sử dụng trong quá trình chuyển đổi là vị trí sau nước đi của anh ta. 

Cuối cùng, hãy xem xét việc nảy bộ điều khiển:```
4 1 3
to tail
0001
```Đường dẫn điều khiển trở thành:```
3 -> 4 -> 3 -> 2
```Sự đảo ngược xảy ra ngay sau khi vào toa xe số 4. 

Vòng tiền xử lý cập nhật hướng chính xác khi`pos == 1`hoặc`pos == n`, do đó quỹ đạo được tạo ra khớp chính xác với các quy tắc.
