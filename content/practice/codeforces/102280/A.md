---
title: "CF 102280A - \u041a\u0430\u043c\u0435\u043d\u044c, \u043d\u043e\u0436\u043d\u0438\u0446\u044b, \u0431\u0443\u043c\u0430\u0433\u0430"
description: "Chúng ta có n trình điều khiển, ban đầu được sắp xếp theo vị trí mà chúng chiếm giữ trong nhật ký. Nhật ký là một chuỗi R, S và P, nhưng ranh giới giữa các vòng trò chơi đã bị xóa."
date: "2026-08-13T09:39:28+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102280
codeforces_index: "A"
codeforces_contest_name: "2010, \u0422\u0440\u0435\u043d\u0438\u0440\u043e\u0432\u043a\u0430 \u0421\u0413\u0410\u0423 aka \u041a\u043e\u043d\u0442\u0435\u0441\u0442 \u043f\u0440\u043e \u043c\u0430\u0440\u0448\u0440\u0443\u0442\u043a\u0438"
rating: 0
weight: 102280
solve_time_s: 166
verified: true
draft: false
---

[CF 102280A - \u041a\u0430\u043c\u0435\u043d\u044c, \u043d\u043e\u0436\u043d\u0438\u0446\u044b, \u0431\u0443\u043c\u0430\u0433\u0430](https://codeforces.com/problemset/problem/102280/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 46s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có`n`trình điều khiển, ban đầu được sắp xếp theo vị trí mà chúng chiếm giữ trong nhật ký. Nhật ký là một chuỗi duy nhất`R`,`S`, Và`P`, nhưng ranh giới giữa các vòng chơi đã bị xóa bỏ. 

Một vòng được chơi bởi mọi tay đua hiện còn trong nhóm có liên quan, vì vậy nếu nhóm hiện tại có`k`chính xác là trình điều khiển`k`các ký tự liên tiếp thuộc về vòng đó. Nếu cả ba dấu hiệu xảy ra hoặc tất cả các dấu hiệu đều giống nhau thì không ai bị loại và cùng một nhóm sẽ chơi lại. Nếu xuất hiện đúng hai ký hiệu thì một ký hiệu sẽ thua theo quy tắc oẳn tù tì thông thường. Mọi tài xế có dấu hiệu thua sẽ trở thành ứng cử viên và chỉ những ứng viên đó mới tiếp tục quá trình loại trừ hiện tại. Khi quá trình đó để lại chính xác một tài xế, tài xế đó sẽ rời khỏi tuyến đường và nhóm trước đó sẽ tiếp tục mà không có họ. 

Nhiệm vụ là xây dựng lại quá trình này từ nhật ký và xuất ra số lượng trình điều khiển cuối cùng còn lại. Nếu nhật ký không thể mô tả đầy đủ chuỗi trò chơi hợp pháp, câu trả lời là`FAIL`. 

Khó khăn chính là nhật ký không nói rõ ràng nơi kết thúc vòng đấu. May mắn thay, độ dài của một vòng không phải là không xác định được. Đó chính xác là số lượng tài xế trong nhóm hiện tại. Khi gặp vòng quyết định, dấu thua sẽ quyết định nhóm tiếp theo nên quy mô của nhóm cũng đã được biết trước. 

Số lượng tài xế nhiều nhất là`100`, do đó trạng thái mô tả trò chơi hiện tại là rất nhỏ. Tuy nhiên, nhật ký có thể chứa tới một triệu ký tự. Điều này loại trừ mọi thứ theo cấp số nhân trong độ dài nhật ký và thực hiện quét tuyến tính nhật ký thành mục tiêu tự nhiên. Giá trị nhỏ của`n`cũng có nghĩa là việc sao chép hoặc lọc một nhóm trình điều khiển sẽ rẻ. 

Một số trường hợp đặc biệt có thể vô hiệu hóa việc triển khai bất cẩn. Ví dụ,```
2
RRRR
```phải sản xuất`FAIL`. Mỗi vòng có hai dấu bằng nhau nên mỗi vòng là một trận hòa và không ai bỏ cuộc. Giải pháp chỉ xử lý các ký tự có sẵn cho đến khi chuỗi kết thúc có thể báo cáo sai trình điều khiển còn lại. 

Một trường hợp khác là một vòng chứa cả ba dấu hiệu:```
3
RSP
```Đây là một trận hòa chứ không phải là loại. Nhật ký kết thúc ngay sau đó nên câu trả lời đúng là`FAIL`. Ở đây coi sự hiện diện của hai dấu hiệu khác nhau là đủ để loại bỏ ai đó là sai lầm. 

Một trường hợp ít rõ ràng hơn là vòng quyết định mà dấu hiệu thua chỉ xuất hiện một lần:```
2
RP
```

`R`thua`P`, vậy tài xế`1`lá và tài xế`2`là người sống sót cuối cùng. Đầu ra đúng là`2`. Giải pháp giả định vòng quyết định luôn để lại ít nhất hai ứng viên sẽ thất bại trong trường hợp này. 

Cuối cùng, một trò chơi hoàn chỉnh không thể có các ký tự nhật ký chưa được sử dụng:```
2
RPRP
```Vòng đầu tiên đã quyết định trận đấu, bởi vì`R`thua`P`. Tài xế`1`lá và tài xế`2`thắng, vì vậy thêm`RP`làm cho nhật ký không hợp lệ. Câu trả lời là`FAIL`. 

## Phương pháp tiếp cận 

Một giải pháp brute-force có thể coi các ranh giới vòng bị thiếu là không xác định và thử mọi phân vùng có thể có của nhật ký thành các vòng, mô phỏng từng phân vùng ứng cử viên. Một chuỗi có độ dài`L`có`2^(L-1)`những cách có thể để đặt ranh giới giữa các ký tự liền kề. Mô phỏng một phân vùng mất`O(L)`, vì vậy cách tiếp cận này cần`O(L * 2^L)`làm việc trong trường hợp xấu nhất. Vì`L = 1,000,000`, thậm chí riêng số lượng phân vùng có thể có cũng lớn hơn về mặt thiên văn so với bất kỳ số lượng hoạt động khả thi nào. 

Sở dĩ lực lượng vũ phu này không cần thiết là vì bản thân trò chơi đã xác định mọi ranh giới. Tại bất kỳ thời điểm nào nhóm hiện tại có kích thước đã biết`k`, do đó vòng tiếp theo phải chiếm đúng vị trí tiếp theo`k`nhân vật. Chúng ta có thể kiểm tra vòng đó ngay lập tức. Một lá hòa`k`không thay đổi. Vòng quyết định cho ta dấu thua, nên việc lọc nhóm hiện tại theo dấu đó sẽ cho ra chính xác nhóm phải tiếp tục loại. 

Có một phần trạng thái mà mô phỏng lặp đơn giản phải bảo toàn. Khi một nhóm thua cuộc nhỏ hơn đang được giải quyết, chúng tôi đã tạm thời rời khỏi nhóm mẹ của nó. Sau khi nhóm nhỏ hơn đó tạo ra một trình điều khiển đã bị loại bỏ, nhóm chính phải tiếp tục với trình điều khiển đó đã bị loại bỏ. Điều này được thể hiện một cách tự nhiên bởi một nhóm các nhóm cha mẹ. 

Thuật toán kết quả sử dụng mỗi ký tự nhật ký chính xác một lần. Các hoạt động liên quan đến nhóm người lái xe chỉ mất`O(n)`theo mức độ loại bỏ, và có nhiều nhất`n-1`sự loại bỏ. Từ`n <= 100`, công việc làm thêm này là không đáng kể. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(L · 2^L)`|`O(L)`| Quá chậm | 
| Tối ưu |`O(L + n^2)`|`O(n^2)`| Đã chấp nhận | 

Đây`L`là độ dài nhật ký và`n`là số lượng người lái xe. 

## Hướng dẫn thuật toán 

1. Bắt đầu với nhóm hiện tại chứa trình điều khiển`1, 2, ..., n`. Duy trì một con trỏ`pos`đến ký tự chưa đọc tiếp theo của nhật ký. Nhóm hiện tại luôn được lưu trữ theo thứ tự mà trình điều khiển của nó viết các dấu hiệu của họ. 
2. Lấy chính xác`len(current)`các ký tự từ nhật ký. Nếu còn ít ký tự hơn, nhật ký sẽ kết thúc ở giữa một vòng, do đó nhật ký không hợp lệ và câu trả lời là`FAIL`. 
3. Xác định những dấu hiệu nào xảy ra ở vòng này. Nếu chỉ có một dấu hiệu phân biệt thì vòng đấu là hòa và nhóm hiện tại không thay đổi. Nâng cao`pos`theo quy mô nhóm và xử lý một vòng khác. 
4. Nếu xuất hiện cả ba dấu hiệu thì vòng đấu cũng được tính là hòa. Một lần nữa tiến lên`pos`theo kích thước nhóm mà không thay đổi nhóm hiện tại. 
5. Nếu xuất hiện đúng 2 biển báo thì xác định biển nào bị mất. Đối với các cặp`R,S`,`S,P`, Và`P,R`, dấu mất tương ứng là`S`,`P`, Và`R`. 
6. Lọc nhóm hiện tại, giữ chính xác các trình điều khiển có dấu bằng dấu bị mất. Đây là những ứng cử viên tiếp tục bị loại. Trước khi thay thế nhóm hiện tại, hãy đẩy nhóm cũ vào ngăn xếp vì nhóm phải tiếp tục sau khi quá trình loại bỏ này kết thúc. 
7. Nếu nhóm mới chứa ít nhất hai trình điều khiển, hãy tiếp tục xử lý nhóm đó theo cách tương tự. Mỗi vòng quyết định sẽ giảm nghiêm ngặt quy mô nhóm, vì vậy quá trình lồng nhau này không thể tiếp tục vô thời hạn. 
8. Nếu nhóm mới có một tài xế, tài xế đó đã được xác định là người mất quyền loại hiện tại và rời khỏi gara. Đưa nhóm chính ra khỏi ngăn xếp và xóa trình điều khiển này khỏi nhóm đó. Nhóm chính sau đó lại trở thành nhóm hiện tại. 
9. Khi nhóm hiện tại có đúng một tài xế và không còn nhóm chính thì tài xế đó là người chiến thắng cuối cùng. Nhật ký chỉ hợp lệ nếu`pos`chính xác là`len(log)`. Bất kỳ ký tự nào còn lại có nghĩa là trò chơi đã ghi có chứa các sự kiện bổ sung, vì vậy câu trả lời là`FAIL`. 

Tại sao nó hoạt động: ở mỗi lần lặp,`current`chính xác là nhóm tay đua có vòng tiếp theo đang được chơi, theo đúng thứ tự ghi. Kích thước của nó xác định ranh giới vòng tiếp theo duy nhất có thể. Một trận hòa khiến nhóm này không thay đổi, trong khi vòng quyết định xác định duy nhất dấu hiệu thua cuộc, do đó, việc lọc theo dấu hiệu đó sẽ đưa ra chính xác các ứng cử viên theo quy định. Khi nhóm ứng cử viên đó tiếp cận một trình điều khiển, việc xóa trình điều khiển đó khỏi nhóm chính đã lưu sẽ tái tạo lại chính xác trạng thái của trò chơi trước khi quá trình loại bỏ đệ quy bắt đầu. Do đó, bất biến vẫn tồn tại sau mỗi quá trình chuyển đổi và khi chỉ còn lại một trình điều khiển cấp gốc thì trình điều khiển đó nhất thiết phải là người chiến thắng cuối cùng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    log = input().strip()

    pos = 0
    current = list(range(1, n + 1))
    stack = []

    while True:
        k = len(current)

        if k == 1:
            winner = current[0]

            if stack:
                parent = stack.pop()
                parent.remove(winner)
                current = parent
                continue

            if pos == len(log):
                return str(winner)

            return "FAIL"

        if pos + k > len(log):
            return "FAIL"

        round_ = log[pos:pos + k]
        pos += k

        signs = set(round_)

        if len(signs) == 1 or len(signs) == 3:
            continue

        if signs == {"R", "S"}:
            loser = "S"
        elif signs == {"S", "P"}:
            loser = "P"
        else:
            loser = "R"

        next_group = [
            player
            for player, sign in zip(current, round_)
            if sign == loser
        ]

        stack.append(current)
        current = next_group

print(solve())
```các`current`list lưu trữ số lượng trình điều khiển thực tế thay vì chỉ số lượng trình điều khiển. Điều này là cần thiết vì câu trả lời cuối cùng phụ thuộc vào danh tính chứ không chỉ đơn thuần là quy mô của nhóm còn lại. 

các`pos`biến luôn trỏ đến ký tự đầu tiên chưa được gán cho vòng. biểu hiện`log[pos:pos + k]`hợp lệ chính xác vì mỗi vòng chứa một dấu hiệu từ mỗi tay đua trong nhóm hiện tại. 

Tập hợp các dấu hiệu phân biệt ba loại hình tròn có thể có. Một dấu hiệu khác biệt có nghĩa là một trận hòa nhất trí, ba dấu hiệu riêng biệt có nghĩa là một trận hòa ba chiều và hai dấu hiệu khác biệt có nghĩa là bị loại. 

Đến vòng quyết định,`zip(current, round_)`liên kết từng dấu hiệu được ghi với trình điều khiển tương ứng. Đang bật lọc`loser`xây dựng nhóm ứng cử viên chính xác cho việc loại bỏ đệ quy. Thứ tự của trình điều khiển được giữ nguyên, điều này quan trọng vì việc đánh số nhật ký dựa trên thứ tự ghi. 

Ngăn xếp lưu trữ các nhóm cha mẹ. Phụ huynh chỉ được đẩy khi vòng quyết định tạo ra một nhóm nhỏ hơn. Khi nhóm nhỏ hơn đó tiếp cận được một trình điều khiển, trình điều khiển đó sẽ bị xóa khỏi cấp độ gốc và cấp độ gốc sẽ tiếp tục lại. Vì độ sâu lồng tối đa chỉ là`n-1`, ngăn xếp được giới hạn an toàn bởi`99`các phần tử. 

Không có vấn đề tràn số nguyên trong Python và tất cả việc lập chỉ mục đều được bảo vệ bởi`pos + k > len(log)`. Sự bình đẳng cuối cùng`pos == len(log)`cũng cần thiết, bởi vì việc tìm ra người chiến thắng thành công không làm cho nhật ký dài quá hợp lệ. 

## Ví dụ đã hoạt động 

Đối với mẫu 1,```
2
RRSSSP
```nhóm ban đầu là trình điều khiển`1`Và`2`. Hai ký tự đầu tiên tạo thành một trận hòa, hai ký tự tiếp theo cũng vậy. Cặp cuối cùng là`SP`, nơi giấy thắng kéo, nên tài xế`2`thua và tài xế`1`vẫn còn. 

| Bước | Nhóm hiện tại | Vòng | Dấu hiệu khác biệt | Hành động | Vị trí | 
| --- | --- | --- | --- | --- | --- | 
| 1 |`[1, 2]`|`RR`|`{R}`| Vẽ | 2 | 
| 2 |`[1, 2]`|`SS`|`{S}`| Vẽ | 4 | 
| 3 |`[1, 2]`|`SP`|`{S,P}`|`S`thua, tài xế 2 lá | 6 | 
| 4 |`[1]`| không | không | Người chiến thắng cuối cùng | 6 | 

Vị trí cuối cùng chính xác là độ dài nhật ký, vì vậy nhật ký hợp lệ và câu trả lời là`1`. 

Đối với mẫu 2,```
3
RSPRSRRP
```vòng đầu tiên có cả ba dấu hiệu và là một trận hòa. Vòng thứ hai là`RSR`, nơi kéo mất đi, chỉ còn lại tài xế`2`. Người lái xe đó rời đi nên nhóm mẹ trở thành`[1, 3]`. Vòng tiếp theo của nó là`RP`, nơi đá thua giấy, nên tài xế`1`lá và tài xế`3`trở thành người chiến thắng cuối cùng. 

| Bước | Nhóm hiện tại | Vòng | Dấu hiệu khác biệt | Hành động | Xếp chồng sau hành động | Vị trí | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 |`[1, 2, 3]`|`RSP`|`{R,S,P}`| Vẽ |`[]`| 3 | 
| 2 |`[1, 2, 3]`|`RSR`|`{R,S}`|`S`thua, tiếp tục với`[2]`|`[[1,2,3]]`| 6 | 
| 3 |`[2]`| không | không | Tài xế 2 lá |`[]`| 6 | 
| 4 |`[1, 3]`|`RP`|`{R,P}`|`R`thua, tài xế 1 lá |`[]`| 8 | 
| 5 |`[3]`| không | không | Người chiến thắng cuối cùng |`[]`| 8 | 

Một lần nữa toàn bộ nhật ký được sử dụng và trình điều khiển còn lại được`3`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(L + n^2)`| Mỗi ký tự nhật ký được xử lý một lần, đồng thời lọc và loại bỏ chi phí trình điều khiển nhiều nhất`O(n^2)`tổng thể | 
| Không gian |`O(n^2)`| Ngăn xếp chứa các nhóm lồng nhau, mỗi nhóm có kích thước tối đa`n`| 

Với`L <= 1,000,000`Và`n <= 100`, công việc chủ yếu là một lần chuyển qua nhật ký. Trạng thái liên quan đến trình điều khiển rất nhỏ nên giải pháp vừa vặn thoải mái với giới hạn bộ nhớ 64 MB. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve_data(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)
    try:
        n = int(input())
        log = input().strip()

        pos = 0
        current = list(range(1, n + 1))
        stack = []

        while True:
            k = len(current)

            if k == 1:
                winner = current[0]

                if stack:
                    parent = stack.pop()
                    parent.remove(winner)
                    current = parent
                    continue

                if pos == len(log):
                    return str(winner)

                return "FAIL"

            if pos + k > len(log):
                return "FAIL"

            round_ = log[pos:pos + k]
            pos += k

            signs = set(round_)

            if len(signs) == 1 or len(signs) == 3:
                continue

            if signs == {"R", "S"}:
                loser = "S"
            elif signs == {"S", "P"}:
                loser = "P"
            else:
                loser = "R"

            next_group = [
                player
                for player, sign in zip(current, round_)
                if sign == loser
            ]

            stack.append(current)
            current = next_group
    finally:
        sys.stdin = old_stdin

def run(inp: str) -> str:
    return solve_data(inp)

assert run("2\nRRSSSP\n") == "1", "sample 1"
assert run("3\nRSPRSRRP\n") == "3", "sample 2"

assert run("2\nRP\n") == "2", "minimum-size game"
assert run("2\nRRRR\n") == "FAIL", "only draws, no winner"
assert run("3\nRSP\n") == "FAIL", "three-way draw with incomplete game"
assert run("2\nRPRP\n") == "FAIL", "extra data after the winner"

long_log = "R" * (9949 * 100)
for size in range(100, 1, -1):
    long_log += "R" * (size - 1) + "S"

assert len(long_log) == 999949
assert run("100\n" + long_log + "\n") == "1", "maximum-size log"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2\nRP`|`2`| Số lượng trình điều khiển tối thiểu và loại bỏ ngay lập tức | 
|`2\nRRRR`|`FAIL`| Các trận hòa dấu bằng lặp đi lặp lại không thể kết thúc trò chơi | 
|`3\nRSP`|`FAIL`| Vòng ba ký là hòa chứ không phải loại trừ | 
|`2\nRPRP`|`FAIL`| Các ký tự bổ sung sau người chiến thắng cuối cùng sẽ làm mất hiệu lực nhật ký | 
|`100`với một`999949`-nhật ký ký tự |`1`| Số lượng trình điều khiển tối đa và nhật ký gần đạt giới hạn một triệu ký tự | 

Bài kiểm tra kích thước tối đa bao gồm`9949`kích thước vòng nhất trí`100`, tiếp theo là các vòng loại bỏ kích thước`100, 99, ..., 2`. Mỗi vòng loại trừ có một`S`và mặt khác`R`, Vì thế`S`thua và người lái xe cuối cùng trong nhóm hiện tại bị loại bỏ. Người sống sót cuối cùng là tài xế`1`. 

## Vỏ cạnh 

Đối với trường hợp đều bằng nhau,```
2
RRRR
```thuật toán tiêu thụ`RR`, nhận ra một dấu hiệu riêng biệt và giữ`[1, 2]`. Sau đó nó tiêu thụ một thứ khác`RR`và về đích với hai tài xế vẫn có mặt. Vì không có nhóm mẹ và không có người chiến thắng duy nhất nên thuật toán trả về`FAIL`. 

Đối với trận hòa ba bên,```
3
RSP
```nhóm hiện tại là`[1, 2, 3]`, và vòng duy nhất là`RSP`. Tập hợp các dấu hiệu của nó có kích thước bằng ba, do đó thuật toán giữ nguyên nhóm. Nhật ký kết thúc trong khi vẫn còn ba trình điều khiển, tạo ra chính xác`FAIL`. 

Đối với nhóm thua đơn,```
2
RP
```vòng này chứa`R`Và`P`. Rock thua nên nhóm tiếp theo có tài xế`1`. Nhóm hiện tại đã đạt kích thước một, vì vậy tài xế`1`bị xóa khỏi cha mẹ của nó`[1, 2]`. Cha mẹ trở thành`[2]`và vì nhật ký đã được sử dụng hết nên trình điều khiển`2`được trả về là người chiến thắng. 

Đối với các ký tự phụ,```
2
RPRP
```cái đầu tiên`RP`đã loại bỏ trình điều khiển`1`, rời khỏi tài xế`2`với tư cách là người sống sót cuối cùng. Sau đó thuật toán sẽ kiểm tra`pos`dựa vào độ dài nhật ký và tìm thấy hai ký tự không được sử dụng. Vì một trò chơi hoàn chỉnh hợp pháp không thể chứa các sự kiện sau khi người chiến thắng đã được xác định, nên nó sẽ trả về`FAIL`. 

Trường hợp có độ dài tối đa nhấn mạnh một ranh giới khác. Thuật toán không bao giờ giả định rằng độ dài nhật ký gần bằng`n`và nó không bao giờ cố gắng liệt kê các ranh giới tròn có thể. Mỗi vòng sử dụng chính xác kích thước của nhóm hiện tại, do đó, thậm chí gần một triệu ký tự được xử lý bởi cùng một quá trình chuyển đổi trạng thái xác định.
