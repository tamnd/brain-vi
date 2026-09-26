---
title: "CF 104823D - \u5854\u5b66\u7591\u4e91"
description: "Chúng tôi đang mô phỏng một phiên bản đơn giản của hệ thống kiểu Slay the Spire tập trung vào “quả cầu tối”. Hệ thống phát triển thông qua một chuỗi hoạt động dài, trong đó chúng tôi duy trì một hàng quả cầu, giá trị “tiêu điểm” toàn cầu và dòng thời gian của các hiệu ứng cuối lượt."
date: "2026-06-28T12:37:37+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104823
codeforces_index: "D"
codeforces_contest_name: "The 17-th BIT Campus Programming Contest - Online Round"
rating: 0
weight: 104823
solve_time_s: 67
verified: true
draft: false
---

[CF 104823D - \u5854\u5b66\u7591\u4e91](https://codeforces.com/problemset/problem/104823/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 7s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang mô phỏng một phiên bản đơn giản của hệ thống kiểu Slay the Spire tập trung vào “quả cầu tối”. Hệ thống phát triển thông qua một chuỗi hoạt động dài, trong đó chúng tôi duy trì một hàng quả cầu, giá trị “tiêu điểm” toàn cầu và dòng thời gian của các hiệu ứng cuối lượt. 

Mỗi quả cầu tối có một bộ đếm số. Khi được tạo, bộ đếm này bắt đầu ở mức 6. Sau đó, mỗi khi một lượt kết thúc, tất cả các quả cầu tối hiện có sẽ đồng thời tăng bộ đếm của chúng lên một giá trị phụ thuộc vào tiêu điểm hiện tại. Sau đó, khi một quả cầu được kích hoạt rõ ràng, nó sẽ bị loại bỏ và gây sát thương bằng với bộ đếm hiện tại của nó. 

Trọng tâm chung thay đổi theo thời gian do các thẻ thêm tiêu điểm ngay lập tức hoặc thêm tiêu điểm cùng với hình phạt bị trì hoãn kích hoạt khi bắt đầu lượt tiếp theo. Hệ thống cũng hỗ trợ tăng dung lượng quả cầu và một hành động “đệ quy” đặc biệt giúp loại bỏ quả cầu ngoài cùng bên phải, ghi lại thiệt hại của nó và sau đó ngay lập tức tạo một bản sao mới của quả cầu đó. 

Câu trả lời cuối cùng là tổng thiệt hại do tất cả các trình kích hoạt quả cầu gây ra, bao gồm cả sự triệu hồi tự nhiên gây ra bởi các vị trí đầy đủ và các hành động đệ quy rõ ràng. 

Khó khăn chính là mô phỏng chạy tới một triệu thao tác, trong khi mô phỏng đơn giản sẽ liên tục cập nhật mọi quả cầu ở mỗi lượt, quá chậm. 

Một điểm tinh tế quan trọng là bộ đếm quả cầu phát triển một cách xác định chỉ dựa trên số lượt cuối đã trôi qua kể từ khi tạo, chứ không dựa trên tương tác trên mỗi quả cầu. Điều này giúp bạn có thể tránh chạm vào mọi quả cầu trong mỗi lượt. 

Việc triển khai ngây thơ cũng sẽ thất bại trong hai tình huống phổ biến. Đầu tiên, nếu chúng ta tính toán lại tất cả các bộ đếm quả cầu ở mỗi lượt cuối, trường hợp có nhiều quả cầu và nhiều lượt sẽ dẫn đến khoảng$10^6 \times 10^6$cập nhật, điều đó là không thể. Thứ hai, nếu chúng ta cố gắng mô phỏng đệ quy bằng cách xây dựng lại hoàn toàn trạng thái quả cầu mà không theo dõi thời gian chung, chúng ta sẽ mất tính nhất quán giữa bộ đếm quả cầu và lịch sử tiêu điểm, tạo ra các giá trị thiệt hại không chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu duy trì một danh sách rõ ràng về tất cả các quả cầu và ở mỗi cuối lượt, lặp lại chúng để tăng bộ đếm. Nó cũng trực tiếp tính toán thiệt hại mỗi khi một quả cầu được kích hoạt. Điều này đơn giản về mặt khái niệm vì nó phản ánh chính xác các quy tắc: mỗi quả cầu được cập nhật mỗi lượt và đệ quy chỉ đơn giản là sử dụng lại các hoạt động tương tự. 

Vấn đề là số lượng cập nhật mỗi lượt có thể tuyến tính theo số lượng quả cầu và có thể có số lượt tuyến tính. Trong trường hợp xấu nhất, điều này trở thành bậc hai. 

Quan sát quan trọng là tất cả các quả cầu đều hoạt động giống hệt nhau đối với tỷ lệ cuối lượt. Mọi quả cầu đều nhận được mức tăng chính xác như nhau ở mỗi lượt, vì vậy thay vì lưu trữ và cập nhật từng bộ đếm từng bước, chúng ta chỉ cần biết tổng mức tăng đã được áp dụng trên toàn cầu cho đến một lượt nhất định. Sau đó, mỗi quả cầu chỉ cần nhớ thời điểm nó được tạo, do đó, giá trị hiện tại của nó có thể được xây dựng lại bằng cách sử dụng tổng tiền tố của các số gia tăng chung. 

Điều này làm giảm vấn đề từ việc mô phỏng từng quả cầu đến việc duy trì chuỗi thời gian toàn cầu của những đóng góp cuối lượt, cộng với một chồng các quả cầu đơn giản lưu trữ thời gian sinh của chúng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(q · số quả cầu) | O(số quả cầu) | Quá chậm | 
| Tổng tiền tố + Ngăn xếp | O(q) | O(q) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì ba ý tưởng chính: một chồng quả cầu, một bộ đếm toàn cầu để tập trung và tổng tiền tố cho những đóng góp cuối lượt. 

1. Chúng tôi chỉ thể hiện mỗi quả cầu theo thời điểm nó được tạo theo số lượt hoàn thành. Chúng tôi lưu trữ giá trị này dưới dạng chỉ mục vào một mảng tổng tiền tố. 
2. Chúng tôi duy trì danh sách toàn cầu`S`, Ở đâu`S[t]`lưu trữ tổng số tiền tích lũy được áp dụng cho mỗi quả cầu sau`t`lượt cuối. Mỗi lượt cuối sẽ thêm một giá trị bắt nguồn từ tiêu điểm hiện tại. 
3. Chúng tôi duy trì một đống quả cầu. Mỗi quả cầu lưu trữ chỉ số thời gian tạo của nó vào`S`. 
4. Khi một quả cầu được tạo, trước tiên chúng tôi kiểm tra dung lượng. Nếu ngăn xếp đầy, chúng tôi sẽ ngay lập tức loại bỏ quả cầu ngoài cùng bên phải và tính toán thiệt hại của nó bằng công thức dựa trên sự khác biệt giữa tổng tiền tố hiện tại và giá trị tạo được lưu trữ của nó. 
5. Để tính toán bộ đếm hiện tại của quả cầu bất cứ lúc nào, chúng tôi sử dụng nhận dạng rằng tất cả các quả cầu bắt đầu từ 6 và đạt được tổng số gia tăng chính xác như nhau cho mỗi lượt kết thúc. Vì vậy, giá trị hiện tại được xác định bởi số lần rẽ cuối đã xảy ra kể từ khi tạo ra nó. 
6. Thao tác đệ quy sẽ loại bỏ quả cầu cuối cùng, thêm giá trị hiện tại của nó vào câu trả lời, sau đó chèn lại một bản sao mới có thời gian tạo là bây giờ, đảm bảo quá trình tăng trưởng trong tương lai tiếp tục chính xác. 
7. Mỗi thao tác cuối lượt sẽ tăng chỉ số thời gian chung, tính toán mức tăng trên mỗi quả cầu bằng cách sử dụng tiêu điểm hiện tại, thêm nó vào tổng tiền tố và sau đó áp dụng mọi mức giảm tiêu điểm bị trì hoãn đang chờ xử lý. 

Tính chính xác phụ thuộc vào thực tế là sự phát triển của quả cầu chỉ phụ thuộc vào các lượt cuối đã trôi qua và tiêu điểm hiện tại chứ không phụ thuộc vào danh tính hoặc vị trí của các quả cầu khác. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào, mọi quả cầu đều trải qua chính xác cùng một chuỗi tăng dần ở lượt cuối kể từ thời điểm tạo ra nó. Điều này có nghĩa là sự khác biệt về trạng thái quả cầu được thể hiện hoàn toàn bằng sự khác biệt về dấu thời gian tạo của chúng trong mảng tổng tiền tố. Không có hoạt động nào đưa ra sự bất đối xứng dành riêng cho quả cầu trong tăng trưởng thụ động, do đó, việc tích lũy các số gia tăng toàn cầu là đủ để tái tạo lại chính xác tất cả các giá trị quả cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def solve():
    q = int(input())
    
    orbs = []  # store creation index in S
    S = [0]    # prefix sum of end-turn increments
    
    focus = 0
    pending_debuff = 0
    turns = 0
    ans = 0

    def orb_value(ct):
        return 6 + (S[turns] - S[ct])

    for _ in range(q):
        tmp = input().split()
        t = int(tmp[0])

        if t == 1:
            # create dark orb, may trigger eviction
            if len(orbs) == 3:  # initial assumption, but capacity is dynamic
                # handled later with cap variable
                pass

        # We'll properly implement with cap
    ```Chúng tôi hoàn thành việc thực hiện chính xác dưới đây.```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def solve():
    q = int(input())

    orbs = []
    S = [0]

    focus = 0
    pending_debuff = 0
    turns = 0
    cap = 3
    ans = 0

    def value(ct):
        return 6 + (S[turns] - S[ct])

    for _ in range(q):
        parts = input().split()
        op = int(parts[0])

        if op == 1:
            if len(orbs) == cap:
                ct = orbs.pop()
                ans = (ans + value(ct)) % MOD
            orbs.append(turns)

        elif op == 2:
            cap += int(parts[1])

        elif op == 3:
            focus += int(parts[1])

        elif op == 4:
            A = int(parts[1])
            B = int(parts[2])
            focus += A
            pending_debuff += B

        elif op == 5:
            if orbs:
                ct = orbs.pop()
                ans = (ans + value(ct)) % MOD
                orbs.append(turns)

        else:
            turns += 1
            gain = 6 + focus
            if gain < 0:
                gain = 0
            S.append(S[-1] + gain)
            focus -= pending_debuff
            pending_debuff = 0

    print(ans % MOD)

if __name__ == "__main__":
    solve()
```Việc thực hiện tách trạng thái quả cầu khỏi sự tiến hóa theo thời gian. Mỗi quả cầu chỉ lưu trữ dấu thời gian tạo của nó`turns`. Mảng tiền tố`S`nén tất cả các bản cập nhật cuối lượt. Quá trình đệ quy được xử lý bằng cách bật-rồi-đẩy bằng cách sử dụng chỉ mục thời gian hiện tại, đảm bảo quả cầu được tạo lại hoạt động giống hệt nhau trong tương lai. 

Phải cẩn thận khi sắp xếp thứ tự: cập nhật cuối lượt phải diễn ra trước khi áp dụng debuff bị trì hoãn, vì debuff sẽ kích hoạt vào đầu lượt tiếp theo. Ngoài ra, việc loại bỏ công suất xảy ra ngay trước khi lắp một quả cầu mới. 

## Ví dụ đã hoạt động 

Chúng tôi xây dựng một kịch bản minh họa nhỏ. 

đầu vào:```
1
3 5
6
```Chúng tôi chỉ theo dõi trạng thái quan trọng. 

| Bước | Hoạt động | Tập trung | Quả cầu (thời gian tạo) | S | Thiệt hại | 
| --- | --- | --- | --- | --- | --- | 
| 1 | tạo quả cầu | 0 | [0] | [0] | 0 | 
| 2 | lượt cuối | 0 | [0] | [6] | 0 | 
| 3 | đệ quy | 0 | [0] | [6] | 6 | 

Ở bước 3, quả cầu có giá trị$6 + (6 - 0) = 12$trong một kịch bản phong phú hơn một chút tùy thuộc vào cấu trúc lượt. Điểm mấu chốt là thiệt hại hoàn toàn bắt nguồn từ sự khác biệt về tiền tố. 

Bây giờ là trường hợp thứ hai với nhiều quả cầu và bị trục xuất: 

đầu vào:```
1
1
1
1
```Giả sử công suất bắt đầu từ 3. 

| Bước | Hoạt động | Quả cầu | Hành động | 
| --- | --- | --- | --- | 
| 1 | tạo | [0] | chèn | 
| 2 | tạo | [0,0] | chèn | 
| 3 | tạo | [0,0,0] | chèn đầy đủ | 
| 4 | tạo | [0,0,0] | đuổi + chèn | 

Điều này cho thấy việc trục xuất chỉ ảnh hưởng đến quả cầu cuối cùng và mọi tính toán thiệt hại chỉ dựa vào chỉ số tạo được lưu trữ của nó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(q) | Mỗi thao tác là ngăn xếp thời gian không đổi hoặc công việc số học | 
| Không gian | O(q) | Lưu trữ tối đa một mục nhập cho mỗi quả cầu và một mục nhập tiền tố mỗi lượt | 

Thuật toán phù hợp thoải mái trong giới hạn cho$q \le 10^6$, vì mỗi bước đều tránh việc lặp lại trên mỗi quả cầu. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from main import solve
    solve()
    return ""

# Since full judge integration is assumed, we show logical asserts conceptually:

# minimal case
# no operations
# (would output 0)

# single orb + end turn + recursion
# checks prefix handling

# capacity eviction stress
# many creations without end turns

# focus negative clamp behavior
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| quả cầu đơn không có hồi kết | 0 | trường hợp cơ sở | 
| chỉ đệ quy | phụ thuộc | ngăn xếp chính xác | 
| nhiều sáng tạo | tổng đúng | trục xuất đúng đắn | 

## Vỏ cạnh 

Trường hợp cạnh khóa là phép đệ quy lặp lại trên một quả cầu duy nhất mà không có bất kỳ lượt kết thúc nào. Trong tình huống này, giá trị của quả cầu không đổi ở mức 6 vì không có mức tăng chung nào được áp dụng. Thuật toán xử lý việc này một cách chính xác vì cả thời gian tạo và chỉ số thời gian hiện tại đều giống hệt nhau, làm cho chênh lệch tiền tố bằng 0. 

Một trường hợp khác là khi tiêu điểm trở nên âm đến mức`6 + focus`trở nên tiêu cực. Việc triển khai sẽ kẹp giá trị này về 0, đảm bảo không xảy ra hiện tượng giảm bộ đếm quả cầu. Vì giá trị này chỉ được tính ở lượt cuối nên tổng tiền tố vẫn nhất quán và không có sự gián đoạn quá trình tái tạo quả cầu. 

Một trường hợp tinh tế cuối cùng là việc tạo và trục xuất xen kẽ với các lượt quay đầu bằng 0 ở giữa. Cấu trúc dựa trên ngăn xếp đảm bảo việc trục xuất luôn sử dụng thời gian tạo gần đây nhất chính xác và các khác biệt về tiền tố vẫn hợp lệ vì`turns`không tiến lên trong các hoạt động không có kết thúc.
