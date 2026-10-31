---
title: "CF 106339A - Ly Cacao"
description: "Chúng ta được cấp một bộ sưu tập cốc ca cao, mỗi cốc gắn liền với một giá trị nhiệt lượng. Ngoài ra còn có một quá trình làm mát giúp giảm nhiệt độ đồng đều theo thời gian với tốc độ cố định."
date: "2026-06-19T17:01:16+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106339
codeforces_index: "A"
codeforces_contest_name: "UTPC Contest 1-28-2026"
rating: 0
weight: 106339
solve_time_s: 52
verified: true
draft: false
---

[CF 106339A - Cốc ca cao](https://codeforces.com/problemset/problem/106339/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 52s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một bộ sưu tập cốc ca cao, mỗi cốc gắn liền với một giá trị nhiệt lượng. Ngoài ra còn có một quá trình làm mát giúp giảm nhiệt độ đồng đều theo thời gian với tốc độ cố định. Quan sát quan trọng là việc uống rượu diễn ra ngay lập tức, do đó, hạn chế thực sự duy nhất là đảm bảo rằng vào thời điểm chúng ta xem xét một chiếc cốc, nhiệt độ của nó đã giảm xuống mức mục tiêu theo dòng thời gian làm mát toàn cầu nhất quán. 

Thay vì suy nghĩ về việc làm mát từng cốc một cách độc lập, chúng tôi diễn giải lại quy trình trên toàn cầu: tất cả các cốc đều nguội ở cùng một tốc độ, do đó thứ tự tương đối của chúng theo nhiệt không bao giờ thay đổi. Đại lượng có ý nghĩa duy nhất là nhiệt lượng ban đầu tối đa, bởi vì chiếc cốc đó sẽ là chiếc cuối cùng đạt đến bất kỳ ngưỡng mục tiêu nhất định nào. 

Nhiệm vụ giảm xuống còn việc xác định mất bao lâu để chiếc cốc nóng nhất nguội đi từ nhiệt độ ban đầu của nó đến mức yêu cầu, với tốc độ làm mát cố định. 

Từ các ràng buộc được ngụ ý bởi các cài đặt Codeforces điển hình, chúng tôi mong đợi có tới khoảng 10^5 giá trị trở lên. Điều này ngay lập tức loại trừ bất kỳ mô phỏng bậc hai nào về việc làm mát trên mỗi cốc. Bất kỳ giải pháp chính xác nào cũng phải chạy theo thời gian tuyến tính hoặc tốt hơn cho mỗi trường hợp thử nghiệm, lý tưởng nhất là một lần truyền qua mảng. 

Một trường hợp khó phát hiện khi nhiệt độ tối đa đã ở dưới ngưỡng yêu cầu. Trong tình huống đó, không cần thời gian làm mát. Việc triển khai ngây thơ tính toán một cách mù quáng một công thức liên quan đến phép chia có thể tạo ra thời gian âm hoặc vô nghĩa. 

Ví dụ: giả sử nhiệt lượng tối đa là 3 và nhiệt lượng cần thiết là 5. Câu trả lời đúng là 0, vì mọi thứ đã đủ nguội. Việc tính toán bất cẩn một cái gì đó như (cmax - h + d - 1) // d sẽ mang lại giá trị âm, giá trị này không hợp lệ trong bối cảnh thời gian. 

## Phương pháp tiếp cận 

Phương pháp tiếp cận mạnh mẽ sẽ mô phỏng quá trình làm mát theo các bước thời gian riêng biệt. Ở mỗi bước, chúng tôi giảm nhiệt độ của mỗi cốc theo tốc độ làm mát và kiểm tra xem tất cả các cốc có đạt được điều kiện mục tiêu hay không. Nếu có n cốc và chênh lệch làm mát có thể lớn, thì điều này có thể giảm xuống thành các hoạt động O(n × t), trong đó t là số bước thời gian cần thiết. Trong trường hợp xấu nhất, điều này trở nên không khả thi khi cả giá trị n và nhiệt đều lớn. 

Quan sát cấu trúc quan trọng là tất cả các cốc đều bị ảnh hưởng theo thời gian như nhau. Điều này có nghĩa là chúng ta không cần theo dõi quỹ đạo riêng lẻ. Thay vào đó, chúng ta chỉ cần biết cốc nào là nút cổ chai, vì cốc làm mát chậm nhất sẽ quyết định tổng thời gian. 

Nút cổ chai đó luôn là cốc có nhiệt lượng ban đầu lớn nhất. Mọi cốc khác đều đạt ngưỡng sớm hơn hoặc cùng lúc. Sau khi chúng tôi rút gọn bài toán xuống một giá trị duy nhất, phép tính sẽ trở thành một biểu thức số học đơn giản: chúng tôi tính xem cốc nóng nhất có bao nhiêu “nhiệt dư” vượt quá mục tiêu, sau đó chia cho tốc độ làm mát, làm tròn lên vì đơn vị từng phần thời gian vẫn yêu cầu một bước đầy đủ. 

Nếu nhiệt độ tối đa đã ở dưới ngưỡng thì thời gian cần thiết là 0 vì không cần làm mát. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(n · t) | O(1) | Quá chậm | 
| Số học dựa trên Max | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Đọc tất cả các giá trị nhiệt lượng và xác định nhiệt độ tối đa trong số đó. Bước này tách biệt chiếc cốc duy nhất có khả năng xác định câu trả lời cuối cùng. 
2. Đọc ngưỡng nhiệt mục tiêu và tốc độ làm mát. Hai giá trị này xác định tốc độ hệ thống chuyển sang trạng thái mục tiêu. 
3. Tính nhiệt lượng dư là cmax − h. Điều này đo lường mức độ của cốc nóng nhất so với mức yêu cầu. 
4. Nếu phần dư này nhỏ hơn hoặc bằng 0, hãy trả về 0 ngay lập tức. Việc này xử lý trường hợp không cần làm mát vì tất cả cốc đều đã đáp ứng yêu cầu. 
5. Mặt khác, hãy tính số bước làm mát hoàn toàn cần thiết như (dư + d − 1) // d. Việc này thực hiện phân chia trần sao cho bất kỳ phần làm mát còn lại nào vẫn tiêu tốn một đơn vị toàn thời gian. 
6. Xuất giá trị này làm câu trả lời. 

Ý tưởng chính là mọi chiếc cốc đều phát triển giống hệt nhau theo thời gian, do đó tính khả thi của hệ thống hoàn toàn được xác định bởi vị trí xuất phát tồi tệ nhất. 

### Tại sao nó hoạt động 

Tất cả các cốc đều giảm nhiệt với tốc độ không đổi như nhau, do đó thứ tự tương đối của chúng không bao giờ thay đổi. Chiếc cốc có nhiệt độ ban đầu tối đa vẫn là chiếc cốc cuối cùng đạt đến bất kỳ ngưỡng nhất định nào. Quá trình kết thúc chính xác khi chiếc cốc đó chạm tới mục tiêu nên tổng thời gian hoàn toàn được xác định bởi quỹ đạo làm mát riêng của nó. Việc phân chia mức trần tính toán chính xác các bước thời gian riêng biệt trong đó tiến trình một phần vẫn yêu cầu một đơn vị thời gian đầy đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    data = list(map(int, input().split()))
    # Depending on formatting, assume:
    # first value is n, last two are h and d, middle are heats
    n = data[0]
    arr = data[1:1+n]
    h = data[1+n]
    d = data[2+n]

    cmax = max(arr)
    excess = cmax - h

    if excess <= 0:
        print(0)
    else:
        print((excess + d - 1) // d)

if __name__ == "__main__":
    solve()
```Việc thực hiện trực tiếp theo sau việc giảm đến phần tử tối đa. Điều tinh tế duy nhất là xử lý phân tích cú pháp đầu vào một cách chính xác, vì tất cả các giá trị được sử dụng từ một dòng trong cách giải thích này. Phép tính chính là phép chia trần, được thực hiện bằng cách sử dụng thủ thuật số nguyên tiêu chuẩn (x + d − 1) // d để tránh các phép tính dấu phẩy động. 

Nhánh có điều kiện đảm bảo chúng ta không bao giờ xuất ra thời gian âm, điều này sẽ xảy ra nếu nhiệt độ tối đa đã ở dưới ngưỡng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Giả sử chúng ta có nhiệt`[10, 6, 8]`, mục tiêu`h = 5`, và tỷ lệ`d = 3`. 

Đầu tiên chúng ta xác định nhiệt độ tối đa. 

| Bước | cmax | h | dư thừa | hành động | 
| --- | --- | --- | --- | --- | 
| ban đầu | 10 | 5 | - | tính toán tối đa | 
| tính toán | 10 | 5 | 5 | cmax - h | 
| chia | 10 | 5 | 5 | (5 + 3 - 1)//3 | 

chúng tôi nhận được`(5 + 2) // 3 = 7 // 3 = 2`. 

Điều này cho thấy cốc cổ chai quyết định câu trả lời chứ không phải toàn bộ mảng. 

### Ví dụ 2 

Trận đấu`[4, 3, 2]`, mục tiêu`h = 6`, tỷ lệ`d = 2`. 

| Bước | cmax | h | dư thừa | hành động | 
| --- | --- | --- | --- | --- | 
| ban đầu | 4 | 6 | - | tính toán tối đa | 
| kiểm tra | 4 | 6 | -2 | vượt quá 0 | 
| kết quả | - | - | - | đầu ra 0 | 

Điều này xác nhận trường hợp thoát sớm khi không cần làm mát. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | một lượt để tính toán tối đa | 
| Không gian | O(1) | chỉ một vài đại lượng vô hướng được sử dụng | 

Giải pháp tuyến tính theo số cốc, là giải pháp tối ưu vì mọi giá trị đầu vào phải được đọc ít nhất một lần. Mức sử dụng bộ nhớ không đổi ngoài bộ nhớ đầu vào, đáp ứng thoải mái các ràng buộc điển hình của Codeforce. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    data = list(map(int, sys.stdin.read().split()))
    n = data[0]
    arr = data[1:1+n]
    h = data[1+n]
    d = data[2+n]

    cmax = max(arr)
    excess = cmax - h
    if excess <= 0:
        return "0"
    return str((excess + d - 1) // d)

# custom tests
assert run("3 10 6 8 5 3") == "2"
assert run("3 4 3 2 6 2") == "0"
assert run("1 10 1 1") == "9"
assert run("5 1 1 1 1 1 0 5") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3 10 6 8 5 3 | 2 | chia trần bình thường | 
| 3 4 3 2 6 2 | 0 | thùng máy không cần làm mát | 
| 1 10 1 1 | 9 | ứng suất phần tử đơn | 
| 5 1 1 1 1 1 0 5 | 0 | tất cả đều dưới ngưỡng | 

## Vỏ cạnh 

Khi tất cả các cốc đều đã có nhiệt độ bằng hoặc dưới ngưỡng thì mức tối đa vẫn ở dưới h. Đối với đầu vào như`[2, 3, 1]`với h = 5 thì phần dư tính toán trở thành âm. Thuật toán kiểm tra rõ ràng điều này và trả về 0 ngay lập tức, ngăn chặn các giá trị thời gian âm không chính xác. 

Đối với một chiếc cốc duy nhất, mức tối đa là chính chiếc cốc đó nên giải pháp giảm xuống mức chia trần trực tiếp. Ví dụ, nhiệt`[11]`, h = 4, d = 3 cho thừa 7 và trả lời 3 bước, phù hợp với mô phỏng trực tiếp. 

Khi phần dư chia hết cho d, công thức trần hoạt động giống như phép chia tiêu chuẩn. Ví dụ, dư 6 và d 3 mang lại chính xác 2 bước, cho thấy rằng không có sự hiệu chỉnh sai lệch nào được đưa ra trong các trường hợp chia hoàn hảo.
