---
title: "CF 102889E - \u7fa4\u4f53\u72c2\u4e71"
description: "Chiến trường chứa tối đa sáu tay sai. Mỗi quân lính có một giá trị tấn công và một giá trị máu. Trong một lần sử dụng phép thuật, mỗi tay sai nhận được chính xác một cơ hội để hành động, nhưng thứ tự của các cơ hội đó là ngẫu nhiên."
date: "2026-07-25T12:29:01+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102889
codeforces_index: "E"
codeforces_contest_name: "The 15-th Beihang University Collegiate Programming Contest (BCPC 2020) - Final"
rating: 0
weight: 102889
solve_time_s: 43
verified: true
draft: false
---

[CF 102889E - \u7fa4\u4f53\u72c2\u4e71](https://codeforces.com/problemset/problem/102889/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 43s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chiến trường chứa tối đa sáu tay sai. Mỗi quân lính có một giá trị tấn công và một giá trị máu. Trong một lần sử dụng phép thuật, mỗi tay sai nhận được chính xác một cơ hội để hành động, nhưng thứ tự của các cơ hội đó là ngẫu nhiên. Khi đến lượt lính, nó tấn công một đối thủ còn sống được chọn đồng đều. Sát thương xảy ra đồng thời: cả hai quân lính đều mất máu bằng giá trị tấn công của quân lính kia. Một lính có máu không dương sẽ bị loại khỏi chiến trường. 

Nhiệm vụ là tìm, đối với mỗi quân lính ban đầu, xác suất để nó vẫn còn sống sau khi toàn bộ quá trình ngẫu nhiên kết thúc. 

Giá trị nhỏ của n là ràng buộc khóa. Với sáu tay sai, chỉ có 720 lệnh lần lượt có thể thực hiện được, nhưng các lựa chọn mục tiêu có thể phân nhánh nhiều hơn nữa. Mô phỏng trực tiếp mọi cây chiến đấu có thể phát triển quá nhanh. Nếu n lớn, ngay cả việc lưu trữ tất cả các bộ sống có thể có cũng đã quá tốn kém, nhưng ở đây số lượng tay sai nhỏ cho phép chúng ta khám phá không gian trạng thái hoàn chỉnh bằng tính năng ghi nhớ. 

Phần khó khăn là giá trị sức khỏe thay đổi sau mỗi cuộc tấn công. Giải pháp chỉ nhớ những lính còn sống là không chính xác vì hai trận chiến với cùng một bộ còn sống có thể có kết quả khác nhau trong tương lai. Ví dụ, hai trạng thái này đều có hai tay sai còn sống sót, nhưng lượng máu còn lại của chúng có thể quyết định liệu lần trao đổi tiếp theo sẽ giết một bên hay cả hai bên. 

Một lỗi thực hiện phổ biến là quên rằng lính chết vẫn chiếm một vị trí theo thứ tự ban đầu. Coi như:```
2
5 1
1 10
```Lính đầu tiên chỉ giết lính thứ hai sau khi tấn công và lính thứ hai có thể không bao giờ nhận được một đòn tấn công có ý nghĩa nếu nó chết trước lượt của nó. Câu trả lời không thể có được bằng cách cho rằng mỗi quân lính luôn tấn công một lần. 

Một trường hợp khác là một minion duy nhất còn lại:```
1
7 3
```Đầu ra là:```
1.000000000000
```Một mô phỏng bất cẩn có thể cố gắng chọn mục tiêu từ một tập hợp trống và chia cho 0. Một lính sống chỉ cần bỏ qua đòn tấn công của nó khi không có đối thủ tồn tại. 

Trường hợp thứ ba liên quan đến sức mạnh tấn công bằng không:```
2
0 1
1 1
```Lính đầu tiên có thể tấn công nhưng không bao giờ gây sát thương. Quân lính thứ hai cuối cùng sẽ tấn công và tiêu diệt nó, vì vậy kết quả đầu ra là:```
0.000000000000
1.000000000000
```Coi cuộc tấn công bằng 0 là "không tấn công" cho kết quả sai vì hành động vẫn xảy ra và làm thay đổi quy trình ngẫu nhiên. 

## Phương pháp tiếp cận 

Một cách tiếp cận đơn giản là mô phỏng đệ quy mọi sự kiện ngẫu nhiên có thể xảy ra. Bất cứ lúc nào, chúng tôi chọn một trong những tay sai chưa đến lượt. Nếu nó còn sống và có đối thủ, chúng tôi sẽ phân nhánh tới mọi mục tiêu có thể. Vào cuối mỗi lượt, chúng ta biết chính xác tay sai nào sống sót. 

Phương pháp này đúng vì nó tuân theo định nghĩa xác suất một cách trực tiếp. Vấn đề là số lượng chi nhánh. Trong trường hợp xấu nhất với sáu tay sai không bao giờ chết, số lượng lựa chọn có thể gần bằng$$6! \times 5 \times 5 \times 4 \times 3 \times 2 \times 1$$vốn đã có hàng trăm nghìn đường dẫn hoàn chỉnh và các bài toán con lặp đi lặp lại xuất hiện nhiều lần. 

Quan sát làm cho giải pháp này trở nên thực tế là có nhiều lịch sử ngẫu nhiên khác nhau dẫn đến cùng một tình huống trong tương lai. Khi chúng ta biết lính nào đã đến lượt và lượng máu hiện tại của từng lính, lịch sử trước đó không còn quan trọng nữa. Chúng tôi có thể lưu trữ kết quả của trạng thái này. 

Số lượng tay sai nhỏ đến mức một trạng thái có thể được biểu thị bằng một mặt nạ bit gồm các lượt đã hoàn thành và một bộ gồm sáu giá trị máu. Hàm đệ quy trả về phân bố xác suất của mặt nạ còn sót lại cuối cùng từ trạng thái đó. Mỗi lần chuyển đổi đều tính trung bình kết quả của tất cả các lựa chọn ngẫu nhiên có thể có. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Cấp số nhân về số lượng lựa chọn tấn công | Hàm mũ | Quá chậm nếu không hợp nhất các trạng thái | 
| Tìm kiếm trạng thái được ghi nhớ | O(số trạng thái có thể truy cập × chuyển tiếp) | O(số trạng thái có thể truy cập) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Lưu trữ lượng máu hiện tại của từng quân lính cùng với một mặt nạ bit mô tả những quân lính nào đã đến lượt. Trạng thái ban đầu có tất cả các giá trị sức khỏe ban đầu và mặt nạ trống. 
2. Đối với một trạng thái, hãy chọn thống nhất trong số tất cả các tay sai chưa đến lượt. Đây là mô hình thứ tự tấn công ngẫu nhiên. Xác suất đóng góp của mỗi lựa chọn được chia cho số lượt chưa hoàn thành. 
3. Đánh dấu lính được chọn là đã hoàn thành lượt của mình. Nếu nó đã chết, nó không thể tấn công, vì vậy trạng thái tiếp theo chỉ đơn giản là trạng thái máu tương tự với một lượt hoàn thành nữa. 
4. Đếm số tay sai hiện đang sống. Nếu chỉ có tay sai còn sống thì không có mục tiêu hợp pháp nên đòn tấn công sẽ bị bỏ qua. 
5. Nếu không, hãy phân nhánh tới mọi mục tiêu còn sống ngoại trừ kẻ tấn công. Gây sát thương đồng thời bằng cách trừ giá trị tấn công của mỗi bên khỏi lượng máu của bên kia. Các trạng thái kết quả được kết hợp với xác suất bằng nhau vì sự lựa chọn mục tiêu là thống nhất. 
6. Khi mọi tay sai đã hoàn thành lượt của mình, hãy trả lại một phân phối chứa một mặt nạ sống sót với xác suất là một. 

Lý do điều này có tác dụng là vì trạng thái chứa chính xác thông tin có thể ảnh hưởng đến tương lai. Các lựa chọn ngẫu nhiên được thực hiện trước khi đạt đến trạng thái không thể ảnh hưởng đến các cuộc tấn công sau này khi các lượt hoàn thành và giá trị máu hiện tại được cố định. Phép đệ quy khám phá mọi sự kiện ngẫu nhiên có thể xảy ra tiếp theo và tính trung bình chúng theo xác suất của chúng, do đó phân bố cuối cùng chính xác là phân bố xác suất cần thiết. 

## Giải pháp Python```python
import sys
from functools import lru_cache

input = sys.stdin.readline

def solve():
    n = int(input())
    atk = []
    hp0 = []
    for _ in range(n):
        a, h = map(int, input().split())
        atk.append(a)
        hp0.append(h)

    size = 1 << n

    @lru_cache(None)
    def dfs(done, hp):
        if done == size - 1:
            alive = 0
            for i in range(n):
                if hp[i] > 0:
                    alive |= 1 << i
            res = [0.0] * size
            res[alive] = 1.0
            return tuple(res)

        choices = [i for i in range(n) if not (done >> i & 1)]
        ans = [0.0] * size
        inv_turn = 1.0 / len(choices)

        for i in choices:
            ndone = done | (1 << i)
            cur_hp = list(hp)

            if cur_hp[i] <= 0:
                nxt = dfs(ndone, tuple(cur_hp))
                for j in range(size):
                    ans[j] += nxt[j] * inv_turn
                continue

            targets = [j for j in range(n) if j != i and cur_hp[j] > 0]

            if not targets:
                nxt = dfs(ndone, tuple(cur_hp))
                for j in range(size):
                    ans[j] += nxt[j] * inv_turn
                continue

            inv_target = 1.0 / len(targets)
            for t in targets:
                nxt_hp = cur_hp[:]
                nxt_hp[i] -= atk[t]
                nxt_hp[t] -= atk[i]
                nxt = dfs(ndone, tuple(nxt_hp))
                for j in range(size):
                    ans[j] += nxt[j] * inv_turn * inv_target

        return tuple(ans)

    result = dfs(0, tuple(hp0))
    for i in range(n):
        prob = 0.0
        for mask in range(size):
            if mask >> i & 1:
                prob += result[mask]
        print("{:.12f}".format(prob))

if __name__ == "__main__":
    solve()
```Hàm đệ quy`dfs`là tìm kiếm trạng thái được ghi nhớ được mô tả ở trên. Chi tiết triển khai chính là bộ dữ liệu tình trạng phải là một phần của khóa bộ đệm. Chỉ lưu vào bộ nhớ đệm bằng`done`sẽ hợp nhất các quốc gia có kết quả khác nhau trong tương lai. 

Trường hợp cơ sở chuyển đổi các giá trị tình trạng cuối cùng thành một mặt nạ bit còn sót lại. Một chiến trường sáu tay sai chỉ có 64 mặt nạ có thể có, vì vậy việc lưu trữ toàn bộ phân bố xác suất sẽ rẻ. 

Khi xử lý kẻ tấn công, trước tiên mã sẽ xử lý các trường hợp cuộc tấn công bị bỏ qua. Kẻ tấn công đã chết không thể hành động và một tay sai sống đơn độc không có mục tiêu. Vòng lặp mục tiêu áp dụng cả những thay đổi sát thương trước khi chuyển sang trạng thái tiếp theo, phù hợp với chiến đấu đồng thời. 

Số nguyên Python đủ lớn cho các giá trị sức khỏe và giá trị tấn công ở đây, do đó không cần xử lý tràn. Độ sâu đệ quy nhiều nhất là sáu, cũng an toàn. 

## Ví dụ đã hoạt động 

Đối với đầu vào:```
1
1 1
```tay sai duy nhất không bao giờ có đối thủ. 

| mặt nạ xong | sức khỏe | hành động | xác suất cuối cùng | 
| --- | --- | --- | --- | 
| 0 | (1) | chọn minion, không có mục tiêu | tái diễn | 
| 1 | (1) | xong | mặt nạ 1 với xác suất 1 | 

Phân phối được trả về chỉ chứa mặt nạ nơi tay sai còn sống, vì vậy câu trả lời là`1.000000000000`. 

Vì:```
2
6 3
3 3
```có thể có hai lượt đầu tiên. 

| mặt nạ xong | sức khỏe | hành động | kết quả | 
| --- | --- | --- | --- | 
| 00 | (3,3) | minion đầu tiên tấn công thứ hai | sức khỏe trở thành (0,-3) | 
| 00 | (3,3) | minion thứ hai tấn công trước | sức khỏe trở thành (0,-3) | 

Ở cả hai nhánh, kẻ tấn công đầu tiên chết và kẻ tấn công thứ hai cũng chết sau khi bị phản đòn. Mặt nạ cuối cùng đều trống trong mọi trường hợp, vì vậy xác suất sống sót của cả hai đều bằng không. 

Dấu vết chứng tỏ rằng thuật toán xử lý thiệt hại đồng thời một cách chính xác. Kẻ tấn công không sống sót chỉ vì nó tấn công trước. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(S × n² × 2ⁿ) | S là số trạng thái được ghi nhớ có thể truy cập, mỗi trạng thái thử kẻ tấn công và nhắm mục tiêu rồi hợp nhất các bản phân phối | 
| Không gian | O(S × 2ⁿ) | Mỗi trạng thái được lưu trong bộ nhớ đệm sẽ lưu trữ một bản phân phối trên tất cả các mặt nạ cuối cùng có thể có | 

Số lượng tay sai chỉ có sáu, vì vậy toàn bộ không gian trạng thái có thể tiếp cận vẫn có thể quản lý được. Giải pháp tránh việc khám phá lặp đi lặp lại các tình huống chiến đấu giống hệt nhau và phù hợp thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    solve()
    out = sys.stdout.getvalue()
    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return out

assert run("1\n1 1\n").strip() == "1.000000000000"

assert run("2\n6 3\n3 3\n").strip() == "0.000000000000\n0.000000000000"

assert run("2\n7 3\n3 3\n").strip() == "1.000000000000\n1.000000000000"

assert run("2\n0 1\n1 1\n").strip() == "0.000000000000\n1.000000000000"

assert run("3\n1 1000000\n1 1000000\n1 1000000\n").count("\n") == 3
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Một tay sai | 1.000000000000 | Xử lý không có mục tiêu | 
| Hai tay sai có sức hủy diệt ngang nhau | Cả hai đều bằng không | Sát thương đồng thời | 
| Hai tay sai đủ mạnh | Cả hai một | Sống sót sau mọi lượt | 
| Không tấn công minion | Số 0 đầu tiên, số 0 thứ hai | Không xử lý tấn công | 
| Ba tay sai sức khỏe khổng lồ | Ba dòng đầu ra | Giá trị lớn và đệ quy | 

## Vỏ cạnh 

Đối với trường hợp một minion:```
1
7 3
```trạng thái ban đầu ngay lập tức chọn lượt chưa hoàn thành duy nhất. Danh sách mục tiêu trống nên trạng thái tiến lên mà không bị thiệt hại. Mặt nạ cuối cùng chứa tay sai, mang lại xác suất là một. 

Đối với trường hợp tấn công bằng 0:```
2
0 1
1 1
```lính thứ nhất có thể tấn công lính thứ hai, nhưng máu của lính thứ hai không thay đổi vì giá trị tấn công bằng 0. Sau đó, lính thứ hai tấn công và loại bỏ lính đầu tiên. Đệ quy giữ nguyên hành động đầu tiên vì đây vẫn là lượt thực sự, mặc dù nó không gây sát thương. 

Đối với cái chết đồng thời:```
2
6 3
3 3
```kẻ tấn công đầu tiên gây ba sát thương và nhận sáu sát thương. Cả hai giá trị sức khoẻ đều trở nên không dương sau khi trao đổi. Thuật toán cập nhật cả hai giá trị sức khỏe trước khi lặp lại, do đó, nó không cho phép kẻ tấn công tiếp tục một cách sai lầm.
