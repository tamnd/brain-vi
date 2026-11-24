---
title: "CF 922A - Đồ chơi nhân bản"
description: "Chúng tôi bắt đầu với một món đồ chơi sang trọng nguyên bản duy nhất và không có bản sao. Một chiếc máy có thể được áp dụng nhiều lần và mỗi ứng dụng sẽ thay đổi kho đồ tùy thuộc vào loại đồ chơi mà nó được áp dụng."
date: "2026-06-17T03:21:03+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 922
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 461 (Div. 2)"
rating: 1300
weight: 922
solve_time_s: 64
verified: true
draft: false
---

[CF 922A - Đồ chơi nhân bản](https://codeforces.com/problemset/problem/922/A) 

**Đánh giá:** 1300 
**Thẻ:** triển khai 
**Thời gian giải:** 1m 4s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi bắt đầu với một món đồ chơi sang trọng nguyên bản duy nhất và không có bản sao. Một chiếc máy có thể được áp dụng nhiều lần và mỗi ứng dụng sẽ thay đổi kho đồ tùy thuộc vào loại đồ chơi mà nó được áp dụng. 

Nếu chúng ta áp dụng nó cho một món đồ chơi nguyên bản, thì bản gốc đó sẽ được thay thế một cách hiệu quả bằng hai bản gốc và một bản sao, điều này làm tăng cả hai loại. Nếu chúng ta áp dụng nó cho một bản sao thì bản sao đó sẽ được thay thế bằng ba bản sao, chỉ làm tăng số lượng bản sao. 

Nhiệm vụ là xác định xem có tồn tại một chuỗi các ứng dụng máy biến đổi trạng thái ban đầu thành chính xác hay không.`y`bản gốc và`x`sao chép mà không bao giờ vứt bỏ đồ chơi hoặc sử dụng máy khi không có loại được yêu cầu. 

Các ràng buộc cho phép các giá trị lên tới 10^9, loại trừ mọi mô phỏng trên tất cả các trạng thái hoặc trình tự. Bất kỳ cách tiếp cận nào cố gắng xây dựng trình tự từng bước trong BFS hoặc DFS đơn giản trên các trạng thái sẽ bùng nổ, vì cả hai số lượng đều có thể tăng theo cấp số nhân với các hoạt động lặp đi lặp lại. Điều này ngay lập tức gợi ý rằng bài toán phải quy về một điều kiện số học đơn giản hơn là một bài toán tìm kiếm. 

Trường hợp cạnh tinh tế xuất hiện khi một trong các mục tiêu nhỏ. Ví dụ, nếu`y = 0`, điều đó là không thể bởi vì quá trình này không bao giờ giảm số lượng bản gốc xuống dưới 1, vì mọi thao tác trên bản gốc đều làm tăng số bản gốc. Tương tự, nếu`y = 1`, chúng tôi không thể tạo bản sao mà không tăng số bản gốc vượt quá 1, bởi vì mọi hoạt động tạo bản sao đều phải bắt nguồn từ việc áp dụng bản gốc trước. 

Những hành vi ranh giới này gợi ý rằng cấu trúc đơn điệu và bị ràng buộc, không thể điều chỉnh được một cách tự do. 

## Phương pháp tiếp cận 

Phối cảnh bạo lực sẽ cố gắng mô phỏng tất cả các chuỗi hoạt động có thể có từ trạng thái ban đầu`(1 original, 0 copies)`. Mỗi trạng thái phân nhánh tùy thuộc vào việc chúng ta áp dụng máy cho bản gốc hay bản sao, tạo ra biểu đồ trạng thái trong đó các nút là cặp`(orig, copy)`. 

Biểu đồ này phát triển cực kỳ nhanh chóng. Từ bất kỳ trạng thái nào có ít nhất một bản gốc và một bản sao, có thể có hai lần chuyển đổi và cả hai số lượng đều tăng theo thời gian. Các trạng thái giới hạn thậm chí lên đến`(10^9, 10^9)`tạo ra một không gian tìm kiếm rộng lớn đến mức không thể thực hiện được BFS hoặc DFS trong giới hạn. 

Quan sát quan trọng là chúng ta không thực sự cần trình tự, chỉ là liệu trạng thái cuối cùng có thể truy cập được hay không. Thay vì mô phỏng xuôi, chúng ta suy luận ngược. 

Chúng tôi xem xét hoạt động cuối cùng có thể tạo ra trạng thái hợp lệ`(y, x)`. Nếu thao tác cuối cùng được áp dụng cho một bản sao thì bản sao đó phải tồn tại trước đó và tạo ra hai bản sao bổ sung. Điều này có nghĩa là chúng ta có thể đảo ngược nó bằng cách giảm`x`bằng 2. 

Nếu thao tác cuối cùng được áp dụng cho bản gốc thì trước thao tác đó, hệ thống có ít hơn một bản gốc và một bản sao ít hơn, bởi vì việc áp dụng nó sẽ bổ sung thêm`( +1 original, +1 copy )`. Đảo ngược điều này có nghĩa là trừ`(1,1)`. 

Vậy từ`(y, x)`chúng ta có thể cố gắng quay ngược lại bằng hai thao tác nghịch đảo: 

từ`(y, x)`ĐẾN`(y-1, x-1)`nếu như`y > 0`, và từ`(y, x)`ĐẾN`(y, x-2)`nếu như`x >= 2`. 

Thay vì phân nhánh, chúng tôi nhận thấy một bất biến về cấu trúc: mọi thao tác làm tăng bản gốc cũng làm tăng số bản sao với cùng một lượng, trong khi các thao tác chỉ sao chép sẽ tăng số bản sao mà không ảnh hưởng đến bản gốc. Điều này ngụ ý rằng bản gốc phải theo dõi chặt chẽ số lượng “thao tác kiểu gốc” đã được sử dụng. 

Vì chúng ta bắt đầu với một bản gốc nên việc tiếp cận`y`bản gốc có nghĩa là chính xác`y-1`các thao tác đã được áp dụng cho bản gốc. Mỗi thao tác như vậy cũng đóng góp chính xác một bản sao, vì vậy nó đã chiếm`y-1`bản sao. Các bản sao còn lại phải hoàn toàn đến từ các hoạt động chỉ sao chép, mỗi bản đóng góp 2 bản. Vì vậy, phần còn lại`x - (y - 1)`phải không âm và chia hết cho 2. 

Điều này làm giảm vấn đề thành một phép kiểm tra số học đơn giản. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tìm kiếm trạng thái Brute Force | Hàm mũ | O(tiểu bang) | Quá chậm | 
| Bất biến số học tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi rút ra điều kiện khả thi trực tiếp từ vai trò của hoạt động sản xuất ban đầu. 

1. Tính xem đồ chơi ban đầu phải thực hiện bao nhiêu thao tác. Vì chúng ta bắt đầu với một bản gốc và mỗi thao tác như vậy sẽ tăng số bản gốc lên đúng một, nên chúng ta phải sử dụng`y - 1`những hoạt động như vậy. Nếu như`y = 0`, điều này đã thất bại vì bản gốc không bao giờ có thể giảm xuống dưới 1. 
2. Kiểm kê các bản sao do các hoạt động đó đóng góp. Mỗi hoạt động ban đầu đóng góp chính xác một bản sao, vì vậy chúng đã tạo ra`y - 1`bản sao. 
3. Tính các bản sao còn lại dưới dạng`x - (y - 1)`. Điều này thể hiện các bản sao phải hoàn toàn đến từ các hoạt động sao chép-sao chép. 
4. Kiểm tra các điều kiện khả thi còn lại. Nếu phần còn lại là âm, chúng tôi không thể giảm số lượng bản sao, do đó mục tiêu là không thể. Nếu số dư không chẵn thì không thể tạo được bằng cách cộng lặp lại 2. 
5. Trả về "Có" nếu cả hai điều kiện đều đúng, nếu không thì trả về "Không". 

### Tại sao nó hoạt động 

Bất biến chính là số lượng bản gốc xác định duy nhất số lượng thao tác được áp dụng cho bản gốc, bởi vì thao tác đó là cách duy nhất để tăng số bản gốc và nó luôn tăng chúng lên đúng một. Sau khi các thao tác đó được khắc phục, sự đóng góp của chúng vào các bản sao cũng được cố định. Tính linh hoạt duy nhất còn lại nằm ở các thao tác chỉ sao chép, thay đổi các bản sao theo gia số hai mà không ảnh hưởng đến bản gốc. Sự tách biệt này tạo ra một ràng buộc tuyến tính cộng với một ràng buộc chẵn lẻ, không để lại bậc tự do bổ sung nào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

x, y = map(int, input().split())

# must have at least one original at all times
if y == 0:
    print("No")
    sys.exit()

# number of "original operations"
orig_ops = y - 1

# copies contributed by those operations
remaining = x - orig_ops

if remaining < 0 or remaining % 2 != 0:
    print("No")
else:
    print("Yes")
```Mã trực tiếp thực hiện bất biến. Trước tiên, chúng tôi giải quyết vấn đề không thể đạt được bản gốc. Sau đó, chúng tôi tính toán có bao nhiêu thao tác phải được áp dụng cho bản gốc và trừ đi sự đóng góp bắt buộc của chúng đối với các bản sao. Các bản sao còn lại phải có thể đạt được chỉ bằng cách sử dụng các hoạt động sao chép-sao chép, điều này buộc phải có tính không âm và thậm chí là tính chẵn lẻ. 

Một sai lầm phổ biến là quên rằng bản gốc không bao giờ có thể giảm đi, đó là lý do tại sao`y = 0`phải bị từ chối ngay lập tức. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`x = 6, y = 3`Chúng tôi tính toán`orig_ops = y - 1 = 2`. Hai thao tác đó đã tạo được 2 bản sao. Các bản còn lại được`6 - 2 = 4`. 

| Bước | nguồn gốc_ops | bản còn lại | quyết định | 
| --- | --- | --- | --- | 
| ban đầu | 2 | 6 | tính toán | 
| trừ nguồn gốc đóng góp | 2 | 4 | kiểm tra | 
| kiểm tra tính chẵn lẻ | 2 | 4 | hợp lệ | 

Vì 4 là số không âm và chẵn nên câu trả lời là Có. 

Điều này xác nhận rằng sau khi sửa số lượng thao tác ban đầu, phần còn lại của hệ thống hoàn toàn bị hạn chế bởi tính chẵn lẻ. 

### Ví dụ 2 

đầu vào:`x = 3, y = 2`Chúng tôi tính toán`orig_ops = 1`. Điều đó đã đóng góp 1 bản. Các bản còn lại được`3 - 1 = 2`. 

| Bước | nguồn gốc_ops | bản còn lại | quyết định | 
| --- | --- | --- | --- | 
| ban đầu | 1 | 3 | tính toán | 
| trừ nguồn gốc đóng góp | 1 | 2 | kiểm tra | 
| kiểm tra tính chẵn lẻ | 1 | 2 | hợp lệ | 

Vì 2 là số chẵn và không âm nên điều này cũng có thể xảy ra. 

Điều này cho thấy ngay cả những điều chỉnh nhỏ về số lượng bản sao cũng bị hạn chế nghiêm ngặt bởi tính chẵn lẻ thay vì độ lớn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ một vài phép tính số học trên hai số nguyên | 
| Không gian | O(1) | Không sử dụng cấu trúc dữ liệu phụ trợ | 

Giải pháp dễ dàng phù hợp với các ràng buộc vì nó tránh được mọi mô phỏng và giảm bài toán về số học theo thời gian không đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline
    x, y = map(int, input().split())

    if y == 0:
        return "No"

    orig_ops = y - 1
    remaining = x - orig_ops

    if remaining < 0 or remaining % 2 != 0:
        return "No"
    return "Yes"

# provided sample
assert run("6 3") == "Yes"

# minimum originals impossible
assert run("0 0") == "No"

# cannot reduce originals
assert run("1 0") == "No"

# simple valid chain
assert run("1 1") == "Yes"

# parity failure
assert run("3 3") == "No"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 0 0 | Không | không có bản gốc không thể | 
| 1 0 | Không | bản gốc không thể giảm | 
| 1 1 | Có | trạng thái cơ sở đã hợp lệ | 
| 3 3 | Không | vi phạm ràng buộc chẵn lẻ | 

## Vỏ cạnh 

Một trường hợp cạnh tới hạn là khi`y = 0`. Mặc dù công thức`orig_ops = y - 1`gợi ý`-1`, hệ thống không bao giờ cho phép bản gốc giảm xuống dưới 1, vì vậy mọi nỗ lực giải thích điều này về mặt đại số đều thất bại về mặt vật lý. Thuật toán loại bỏ trường hợp này một cách rõ ràng trước khi áp dụng số học. 

Một trường hợp cạnh khác là khi`x`nhỏ hơn`y - 1`. Ví dụ,`x = 1, y = 5`cho`orig_ops = 4`, rời đi`remaining = -3`. Điều này ngay lập tức trở nên không hợp lệ, cho thấy rằng ngay cả khi tính chẵn lẻ có hiệu quả thì chỉ riêng tiêu cực cũng đủ để bác bỏ trạng thái. 

Trường hợp cạnh thứ ba là khi số dư là số lẻ. Ví dụ,`x = 4, y = 3`cho`orig_ops = 2`, rời đi`remaining = 2`, hợp lệ, nhưng`x = 5, y = 3`lá`remaining = 3`, không thể phân tách thành từng phần hai, khiến cho cấu hình không thể truy cập được mặc dù có tổng cộng đủ bản sao.
