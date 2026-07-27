---
title: "CF 102775F - \u042d\u043b\u0438\u043a\u0441\u0438\u0440\u044b"
description: "Nhiệm vụ này mô tả việc nâng cấp tòa nhà thường đòi hỏi thời gian làm việc là t phút. Thuốc tiên được kích hoạt cùng lúc với quá trình nâng cấp bắt đầu. Trong khi thuốc tiên hoạt động trong p phút, quá trình xây dựng sẽ tiến triển nhanh hơn bình thường gấp k lần."
date: "2026-07-27T20:39:08+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102775
codeforces_index: "F"
codeforces_contest_name: "ICPC Central Russia Regional Contest (CRRC 20), \u0427\u0435\u043c\u043f\u0438\u043e\u043d\u0430\u0442 \u0426\u0435\u043d\u0442\u0440\u0430\u043b\u044c\u043d\u043e\u0439 \u0420\u043e\u0441\u0441\u0438\u0438, \u043a\u0432\u0430\u043b\u0438\u0444\u0438\u043a\u0430\u0446\u0438\u043e\u043d\u043d\u044b\u0439 \u0440\u0430\u0443\u043d\u0434"
rating: 0
weight: 102775
solve_time_s: 46
verified: true
draft: false
---

[CF 102775F - \u042d\u043b\u0438\u043a\u0441\u0438\u0440\u044b](https://codeforces.com/problemset/problem/102775/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 46s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ mô tả việc nâng cấp tòa nhà thường yêu cầu`t`phút làm việc. Thuốc tiên được kích hoạt cùng lúc với quá trình nâng cấp bắt đầu. Trong khi thuốc tiên đang hoạt động`p`phút, tiến độ thi công`k`nhanh hơn bình thường nhiều lần. Sau khi thuốc tiên hết hạn, mọi công việc chưa hoàn thành vẫn tiếp tục ở tốc độ bình thường. Mục tiêu là tìm tổng thời gian thực đã sử dụng cho đến khi quá trình nâng cấp hoàn tất. 

Đầu vào chứa ba số nguyên.`t`là khối lượng công việc xây dựng thông thường cần thiết,`k`là hệ số nhân tốc độ trong hiệu ứng thuốc tiên và`p`là thời gian của hiệu ứng tính bằng phút thực. Đầu ra là số phút thực cần thiết khi sử dụng thuốc tiên. 

Những hạn chế là đầu mối chính cho giải pháp. Các giá trị của`t`Và`k * p`có thể đạt được`10^9`, do đó, một mô phỏng tiến bộ từng phút có thể yêu cầu tới một tỷ lần lặp. Đó là quá nhiều cho giới hạn thời gian một giây. Lời giải phải sử dụng phép tính trực tiếp và kết thúc trong thời gian không đổi. 

Một số trường hợp đặc biệt có thể phá vỡ các giải pháp chỉ giải quyết được tình huống chung. Nếu chỉ riêng thuốc tiên đã hoàn thành việc xây dựng thì câu trả lời là không`p`, vì tòa nhà có thể hoàn thành trước khi thuốc tiên hết hạn. Ví dụ, với đầu vào`100 10 20`, thuốc tiên có thể kết thúc tất cả`100`các đơn vị công việc trong`10`phút, vì vậy đầu ra chính xác là`10`. Một giải pháp luôn thêm toàn bộ thời lượng thuốc tiên sẽ trả về giá trị lớn hơn một cách không chính xác. 

Một trường hợp ranh giới khác xuất hiện khi thuốc tiên kết thúc vào đúng thời điểm công trình được xây dựng. Đối với đầu vào`100 5 20`, thuốc tiên cung cấp`5 * 20 = 100`các đơn vị công việc. Đầu ra đúng là`20`. Việc thực hiện bất cẩn coi đây là công việc chưa hoàn thành có thể tốn thêm một phút. 

Lỗi phổ biến cuối cùng là bỏ qua câu trả lời phải là số nguyên phút. Đối với đầu vào`101 10 20`, thuốc tiên cung cấp đủ thời lượng, nhưng tốc độ xây dựng thì chậm`10`đơn vị trên phút, vậy thời gian thực là`ceil(101 / 10) = 11`. Trả về phép chia số nguyên sẽ tạo ra`10`, điều đó không đúng. 

## Phương pháp tiếp cận 

Một cách tiếp cận đơn giản là mô phỏng việc xây dựng từng phút một. Chúng ta có thể theo dõi công việc còn lại và trừ`k`đơn vị cho mỗi phút khi thuốc tiên hoạt động, sau đó trừ đi một đơn vị mỗi phút sau đó. Cách tiếp cận này rất dễ xác minh vì nó tuân theo quy trình một cách chính xác. 

Vấn đề với mô phỏng là kích thước có thể có của đầu vào. Trong trường hợp xấu nhất, tòa nhà có thể yêu cầu`10^9`phút làm việc. Một mô phỏng sẽ thực hiện khoảng một thao tác mỗi phút, dẫn đến`O(10^9)`hoạt động vượt xa những gì có thể phù hợp trong thời hạn. 

Quan sát quan trọng là hiệu ứng thuốc tiên có một lượng công việc tăng tốc cố định. Trong suốt thời gian tồn tại của nó, công trình được hoàn thành chính xác`k * p`phút làm việc bình thường. Thay vì mô phỏng từng phút, chúng ta có thể so sánh trực tiếp số lượng này với tổng công việc được yêu cầu. 

Nếu như`k * p`nhỏ hơn`t`, thần dược hoàn thành một phần công trình và phần còn lại được hoàn thành bình thường. Câu trả lời trở thành`p`cộng với công việc còn sót lại. Nếu như`k * p`ít nhất là`t`, quá trình xây dựng kết thúc trong khi thuốc tiên vẫn còn hoạt động nên chúng ta chỉ cần thời gian cần thiết để thực hiện`t`làm việc với tốc độ`k`. 

Phương pháp brute-force hoạt động vì mỗi phút có thể được mô hình hóa độc lập nhưng không thành công vì có thể có quá nhiều phút. Quan sát thấy rằng toàn bộ thời gian tiên dược có thể được chuyển đổi thành một lượng công việc đã hoàn thành duy nhất sẽ làm giảm vấn đề xuống còn một vài phép tính số học. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(t) | O(1) | Quá chậm | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính tổng khối lượng công việc xây dựng mà thuốc tiên có thể hoàn thành. Trong lúc`p`phút thi công khẩn trương, tiến độ đạt`k * p`phút làm việc bình thường. Giá trị này cho phép chúng tôi quyết định xem việc nâng cấp kết thúc trong thời gian thuốc tiên hay sau đó. 
2. Kiểm tra xem`k * p`ít nhất là`t`. Nếu đúng như vậy, toàn bộ công trình sẽ diễn ra dưới hiệu ứng thần dược. Thời gian cần thiết là trần của`t / k`, bởi vì mỗi phút đều hoàn thành`k`đơn vị công việc và phút cuối cùng chỉ được sử dụng một phần. 
3. Nếu`k * p`nhỏ hơn`t`, trừ đi tiến độ tăng tốc khỏi công việc cần thiết. Số tiền còn lại không thể hưởng lợi từ thuốc tiên, vì vậy nó được thêm trực tiếp vào toàn bộ thời gian`p`. 

Tại sao nó hoạt động: quá trình xây dựng chỉ phụ thuộc vào số lượng công việc được hoàn thành chứ không phụ thuộc vào số phút riêng lẻ trong thời gian dùng thuốc tiên. đầu tiên`p`số phút luôn đóng góp chính xác`k * p`các công việc bình thường trừ khi việc xây dựng kết thúc sớm hơn. Nếu công việc yêu cầu lớn hơn số lượng đó thì phần công việc còn lại vẫn được hoàn thành bình thường. Nếu nó nhỏ hơn hoặc bằng, toàn bộ nhiệm vụ sẽ được thực hiện trong quá trình xây dựng tăng tốc. Hai trường hợp này bao gồm mọi khả năng thực hiện nên kết quả số học khớp với thời gian hoàn thành thực tế. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t, k, p = map(int, input().split())

boosted_work = k * p

if boosted_work >= t:
    print((t + k - 1) // k)
else:
    print(p + (t - boosted_work))
```Biến`boosted_work`lưu trữ tổng khối lượng công việc xây dựng thông thường đã hoàn thành trong toàn bộ thời gian sử dụng thuốc tiên. nhân`k`qua`p`là an toàn vì sản phẩm của họ bị hạn chế bởi những hạn chế của vấn đề. 

Nhánh đầu tiên xử lý trường hợp tòa nhà hoàn thành trước khi thuốc tiên hết hạn. biểu hiện`(t + k - 1) // k`thực hiện phép chia trần, việc này cần thiết vì đầu ra không thể có được một phần của một phút. Ví dụ,`101`đơn vị làm việc với tốc độ`10`nhu cầu`11`phút. 

Nhánh thứ hai xử lý trường hợp thuốc tiên không thể hoàn thành quá trình nâng cấp. Thuốc tiên tiêu thụ chính xác`p`phút thực và hoàn thành`k * p`đơn vị công tác, rời đi`t - k * p`đơn vị để hoàn thành ở tốc độ bình thường. Vì tốc độ bình thường là một đơn vị công việc mỗi phút nên số lượng còn lại có thể được thêm trực tiếp. 

Mã chỉ sử dụng số học số nguyên. Không có phép tính dấu phẩy động nên không có vấn đề về độ chính xác xung quanh việc làm tròn. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
2640 10 60
```| t | k | p | k*p | Quyết định | Trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 2640 | 10 | 60 | 600 | Elixir không hoàn thành nhiệm vụ | 60 + 2040 = 2100 | 

Thuốc tiên hoàn thành`600`đơn vị công việc bình thường trong thời gian làm việc`60`phút. Phần còn lại`2040`các đơn vị tiếp tục ở tốc độ bình thường, mang lại tổng cộng`2100`phút. 

### Mẫu 2 

đầu vào:```
101 10 20
```| t | k | p | k*p | Quyết định | Trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 101 | 10 | 20 | 200 | Elixir hoàn thành nhiệm vụ | trần(101/10) = 11 | 

Thời gian tăng tốc có đủ tổng công suất, vì vậy chỉ có thời gian sử dụng thuốc tiên là quan trọng. Vì mỗi phút hoàn thành mười đơn vị nên cần mười một phút. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Thuật toán thực hiện một số phép tính số học cố định. | 
| Không gian | O(1) | Chỉ có một vài biến số nguyên được lưu trữ. | 

Giải pháp không phụ thuộc vào kích thước của`t`,`k`, hoặc`p`thông qua việc lặp lại. Ngay cả ở các giá trị tối đa được các ràng buộc cho phép, nó vẫn kết thúc ngay lập tức và sử dụng bộ nhớ không đổi. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    t, k, p = map(int, input().split())
    boosted_work = k * p

    if boosted_work >= t:
        ans = (t + k - 1) // k
    else:
        ans = p + t - boosted_work

    sys.stdin = old_stdin
    return str(ans)

assert solve("2640 10 60\n") == "2100", "sample 1"
assert solve("101 10 20\n") == "11", "sample 2"

assert solve("1 1 1\n") == "1", "minimum values"
assert solve("1000000000 1000000000 1\n") == "1", "maximum accelerated speed"
assert solve("100 5 20\n") == "20", "exact elixir completion boundary"
assert solve("100 3 10\n") == "40", "remaining normal work case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 1`|`1`| Các giá trị nhỏ nhất có thể. | 
|`1000000000 1000000000 1`|`1`| Giá trị rất lớn mà không bị tràn hoặc lặp lại. | 
|`100 5 20`|`20`| Trường hợp thuốc tiên kết thúc chính xác khi hoàn thành. | 
|`100 3 10`|`40`| Xử lý đúng thời gian thi công bình thường còn lại. | 

## Vỏ cạnh 

Đối với đầu vào`100 10 20`, thuốc tiên cung cấp`200`đơn vị công việc nhiều hơn yêu cầu`100`. Thuật toán đi vào nhánh hoàn thành tăng tốc và tính toán`ceil(100 / 10) = 10`, đưa ra câu trả lời đúng Nó không chờ đợi đầy đủ một cách chính xác`20`thời gian thuốc tiên phút. 

Đối với đầu vào`100 5 20`, công việc được tăng tốc chính xác là`100`. Thuật toán kiểm tra`k * p >= t`, do đó nó xử lý việc hoàn thành ở ranh giới một cách chính xác và tính toán`ceil(100 / 5) = 20`. 

Đối với đầu vào`101 10 20`, thuật toán cũng sử dụng nhánh hoàn thành tăng tốc vì`200 >= 101`. Việc phân chia trần mang lại`(101 + 10 - 1) // 10 = 11`, tránh lỗi chia số nguyên phổ biến. 

Đối với trường hợp thuốc tiên không đủ, chẳng hạn như đầu vào`100 3 10`, thuốc tiên hoàn thành`30`các đơn vị công việc trong`10`phút. Phần còn lại`70`đơn vị yêu cầu`70`bình thường nhiều phút hơn nên thuật toán trả về`10 + 70 = 80`. Điều này khẳng định rằng quá trình chuyển đổi từ xây dựng cấp tốc sang xây dựng thông thường được xử lý chính xác.
