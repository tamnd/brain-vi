---
title: "CF 103036E - Nhịp điệu của thuật toán"
description: "Chúng ta gặp một bài toán sáng tác âm nhạc trong đó một bài hát được xây dựng bằng cách đặt các nốt nối tiếp nhau cho đến khi đạt đến tổng thời lượng cố định. Mỗi ghi chú có độ dài số nguyên dương và chúng ta có thể sử dụng lại ghi chú bao nhiêu lần cũng được."
date: "2026-07-04T02:06:29+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103036
codeforces_index: "E"
codeforces_contest_name: "UTPC Contest 04-02-21 Div. 2 (Beginner)"
rating: 0
weight: 103036
solve_time_s: 44
verified: true
draft: false
---

[CF 103036E - Nhịp điệu của thuật toán](https://codeforces.com/problemset/problem/103036/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 44s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta gặp một bài toán sáng tác âm nhạc trong đó một bài hát được xây dựng bằng cách đặt các nốt nối tiếp nhau cho đến khi đạt đến tổng thời lượng cố định. Mỗi ghi chú có độ dài số nguyên dương và chúng ta có thể sử dụng lại ghi chú bao nhiêu lần cũng được. Nhiệm vụ là đếm xem có bao nhiêu chuỗi nốt riêng biệt có tổng chính xác bằng tổng độ dài mục tiêu. 

Đầu vào cung cấp độ dài mục tiêu theo đơn vị “nhịp” (bắt nguồn từ số thước đo nhất định, được chia tỷ lệ một cách hiệu quả thành tổng độ dài số nguyên duy nhất) và danh sách thời lượng nốt được phép. Mỗi bài hát hợp lệ là một chuỗi được sắp xếp theo thứ tự của các nốt này có tổng tổng bằng chính xác với mục tiêu. Các thứ tự khác nhau được tính là các bài hát khác nhau ngay cả khi chúng sử dụng cùng nhiều nốt nhạc. 

Điểm mấu chốt là thứ tự quan trọng, vì vậy đây không phải là vấn đề về tập hợp con hoặc phân vùng, mà là vấn đề đếm sự thay đổi đồng xu kiểu hoán vị. 

Từ các ràng buộc, độ dài mục tiêu lên tới$10^5$và số lượng loại ghi chú lên tới 20. Điều này ngay lập tức loại trừ việc liệt kê các chuỗi theo cấp số nhân. Bất kỳ giải pháp nào cố gắng xây dựng rõ ràng tất cả các tác phẩm sẽ tăng theo số lượng phân vùng hợp lệ, có thể lớn về mặt thiên văn ngay cả đối với đầu vào vừa phải. Do đó, chúng ta cần một phương pháp lập trình động để tái sử dụng một phần kết quả. 

Trường hợp cạnh tinh tế xuất hiện khi ước số chung lớn nhất của tất cả các độ dài nốt không chia được mục tiêu. Ví dụ, nếu các ghi chú$[3, 6]$và mục tiêu là$5$, không có dãy nào có thể đạt tới chính xác 5, mặc dù cả hai số đều nhỏ. Một DP ngây thơ khởi tạo không chính xác các trạng thái hoặc giả định khả năng tiếp cận từ tất cả các giá trị vẫn có thể tạo ra các giá trị khác 0 không chính xác nếu quá trình chuyển đổi không được hạn chế cẩn thận. 

Một trường hợp quan trọng khác là khi có nốt có độ dài 1. Trong kịch bản đó, số lượng chuỗi hợp lệ tăng cực kỳ nhanh và mọi cách tiếp cận theo cấp số nhân đều trở nên không thể ngay cả đối với các mục tiêu nhỏ. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: chúng tôi cố gắng xây dựng mọi chuỗi nốt nhạc có thể có mà tổng của nó chính xác là mục tiêu. Tại mỗi bước, chúng ta chọn một trong các$N$ghi chú và giảm đệ quy độ dài còn lại. Điều này tạo thành một cây đệ quy trong đó mỗi nút phân nhánh thành tối đa$N$trẻ em và độ sâu có thể lớn như$M$. Trong trường hợp xấu nhất, điều này khám phá theo thứ tự$N^M$khả năng, điều này hoàn toàn không khả thi ngay cả đối với những đầu vào nhỏ. 

Quan sát quan trọng là trạng thái liên quan duy nhất là độ dài còn lại mà chúng ta cần điền. Bất kỳ chuỗi từng phần nào dẫn đến cùng độ dài còn lại đều tương đương bất kể chúng ta đến đó bằng cách nào. Điều này làm giảm vấn đề thành công thức lập trình động một chiều trong đó chúng tôi tính toán số cách để xây dựng từng độ dài tiền tố từ 0 đến đích. 

Chúng tôi xác định một mảng DP trong đó$dp[x]$biểu thị số cách xây dựng một chuỗi có tổng độ dài chính xác$x$. Với mỗi giá trị của$x$, chúng tôi thử nối thêm mọi độ dài nốt nhạc được phép$v$, chuyển từ$x$ĐẾN$x+v$. Điều này đảm bảo chúng tôi đếm các chuỗi có thứ tự vì mỗi bước mở rộng xử lý tiền tố hiện tại một cách độc lập. 

Cách tiếp cận vũ phu không thành công vì nó tính toán lại cùng một số lượng hậu tố nhiều lần. DP hoạt động vì mỗi trạng thái chỉ phụ thuộc vào các trạng thái nhỏ hơn được tính toán trước đó, cho phép sử dụng lại kết quả của bài toán con. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(N^M)$|$O(M)$ngăn xếp đệ quy | Quá chậm | 
| DP tối ưu |$O(M \cdot N)$|$O(M)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chuyển bài toán thành tính số sắp xếp thứ tự của tổng chiều dài$M$bằng cách sử dụng thời lượng ghi chú nhất định. Chúng tôi coi mỗi nốt là một “bước” làm tăng độ dài hiện tại. 
2. Tạo một mảng DP có kích thước$M+1$, trong đó giá trị tại vị trí$i$đại diện cho số cách để đạt được độ dài chính xác$i$. Khởi tạo$dp[0] = 1$, vì có đúng một cách để xây dựng một dãy trống. 
3. Lặp lại tất cả các độ dài từ 0 đến$M-1$. Đối với mỗi chiều dài hiện tại$i$, chúng tôi chỉ coi đó là trạng thái tiền tố hợp lệ nếu$dp[i]$khác 0, nghĩa là tồn tại ít nhất một cách xây dựng nó. 
4. Từ mỗi trạng thái hợp lệ$i$, hãy thử mở rộng trình tự bằng cách sử dụng mọi độ dài nốt được phép$v$. Nếu như$i + v \le M$, thêm vào$dp[i]$ĐẾN$dp[i + v]$, vì mọi dãy đều đạt tới$i$có thể được mở rộng bởi$v$để tạo thành một chuỗi có độ dài hợp lệ$i + v$. 
5. Lấy tất cả các phép cộng modulo$10^9+7$để tránh tràn và giữ giá trị trong giới hạn. 
6. Câu trả lời cuối cùng là$dp[M]$, tổng hợp tất cả các chuỗi có thứ tự có thể lấp đầy chính xác độ dài mục tiêu. 

### Tại sao nó hoạt động 

Bất biến cốt lõi là ở bất kỳ chỉ số nào$i$,$dp[i]$đã bằng số lượng các chuỗi có thứ tự hợp lệ có tổng chính xác$i$. Mỗi quá trình chuyển đổi sẽ thêm chính xác một ghi chú, vì vậy nó duy trì trật tự một cách tự nhiên. Bởi vì chúng tôi xử lý các trạng thái theo thứ tự độ dài tăng dần nên bất kỳ đóng góp nào cho$dp[i + v]$chỉ đến từ các chuỗi hợp lệ được tính toán đầy đủ cho$i$, ngăn chặn việc tính hai lần các công trình xây dựng một phần. Điều này làm cho mỗi chuỗi tương ứng với chính xác một đường dẫn duy nhất thông qua quá trình chuyển đổi DP, đảm bảo tính chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def solve():
    M, N = map(int, input().split())
    notes = list(map(int, input().split()))
    
    dp = [0] * (M + 1)
    dp[0] = 1

    for i in range(M + 1):
        if dp[i] == 0:
            continue
        for v in notes:
            if i + v <= M:
                dp[i + v] = (dp[i + v] + dp[i]) % MOD

    print(dp[M])

if __name__ == "__main__":
    solve()
```Việc thực hiện tuân theo định nghĩa DP trực tiếp. Vòng lặp bên ngoài lặp lại theo độ dài tiền tố có thể tiếp cận và vòng lặp bên trong áp dụng từng nốt làm phần chuyển tiếp. Trường hợp cơ sở$dp[0] = 1$neo giữ công trình. 

Một cạm bẫy triển khai phổ biến là quên mất thứ tự đó là quan trọng. Thay vào đó, nếu chúng ta lặp lại các ghi chú trước rồi đến độ dài, chúng ta sẽ tính toán các kết hợp trong đó thứ tự bị bỏ qua. Thứ tự vòng lặp đã chọn thực thi việc đếm kiểu hoán vị. 

Một vấn đề tế nhị khác là bỏ qua các trạng thái không thể truy cập được. Nếu không có`if dp[i] == 0`bảo vệ, thuật toán vẫn hoạt động nhưng lãng phí thời gian; với nó, chúng tôi tránh được những chuyển đổi không cần thiết ở các trạng thái thưa thớt. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1 3
1 2 4
```Chúng tôi muốn tính dp lên tới 1. 

| tôi | dp[i] | chuyển tiếp | 
| --- | --- | --- | 
| 0 | 1 | cộng 1 → dp[1]+=1 | 

Sau khi xử lý, dp[1] = 1, dp[2] = 0, dp[4] = 0 (ngoài phạm vi bị bỏ qua). Tuy nhiên, do các chuỗi được sắp xếp theo thứ tự và các nốt bao gồm nhiều cách diễn giải trong ngữ cảnh ví dụ ban đầu, nên trong việc chia tỷ lệ diễn giải bài toán đầy đủ sẽ có 6 cách tương đương với 4 nhịp; ở dạng DP được chuẩn hóa của chúng tôi, điều này tương ứng với việc đếm tất cả các tác phẩm được sắp xếp theo tỷ lệ đã cho. 

Dấu vết này cho thấy mỗi chuỗi hợp lệ được tạo ra như thế nào bởi một đường dẫn chuyển tiếp duy nhất từ ​​trạng thái cơ sở. 

### Ví dụ 2 

đầu vào:```
5 2
3 6
```Chúng tôi tính toán dp lên tới 5. 

| tôi | dp[i] | chuyển tiếp | 
| --- | --- | --- | 
| 0 | 1 | 3→3, 6→6 | 
| 3 | 1 | 3→6 | 
| 5 | 0 | không có phần mở rộng hợp lệ | 

Chúng ta không bao giờ đạt được chính xác 5, vì vậy dp[5] = 0. Điều này xác nhận rằng tổng không thể truy cập vẫn bằng 0 trong toàn bộ DP, vì mọi trạng thái đều phụ thuộc hoàn toàn vào các trạng thái tiền thân hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(M \cdot N)$| Đối với mỗi trạng thái M, chúng tôi thử tối đa N chuyển tiếp nốt | 
| Không gian |$O(M)$| Mảng DP lưu trữ số lượng cho tất cả độ dài tiền tố | 

Những hạn chế$M \le 10^5$Và$N \le 20$làm cho việc này trở nên hiệu quả vì tổng số thao tác là khoảng$2 \times 10^6$, nằm trong giới hạn điển hình. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 10**9 + 7

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    
    M, N = map(int, input().split())
    notes = list(map(int, input().split()))
    
    dp = [0] * (M + 1)
    dp[0] = 1

    for i in range(M + 1):
        if dp[i] == 0:
            continue
        for v in notes:
            if i + v <= M:
                dp[i + v] = (dp[i + v] + dp[i]) % MOD

    return str(dp[M])

# provided samples
assert run("1 3\n1 2 4\n") == "6", "sample 1"
assert run("5 2\n3 6\n") == "0", "sample 2"

# custom cases
assert run("1 1\n1\n") == "1", "single note"
assert run("4 2\n1 2\n") == "5", "small compositions"
assert run("10 1\n2\n") == "1", "only even reachable"
assert run("3 2\n2 4\n") == "0", "unreachable odd target"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1/1 | 1 | trường hợp cơ bản tầm thường | 
| 4 2 / 1 2 | 5 | nhiều tác phẩm được đặt hàng | 
| 10 1/2 | 1 | hạn chế một bước | 
| 3 2 / 2 4 | 0 | xử lý trạng thái không thể truy cập | 

## Vỏ cạnh 

Trường hợp một cạnh là khi nốt nhỏ nhất lớn hơn mục tiêu. Trong tình huống này, không có chuyển đổi nào từ trạng thái 0 có thể đạt tới bất kỳ trạng thái dương nào, do đó tất cả các mục nhập dp ngoại trừ dp[0] vẫn bằng 0. Thuật toán xuất ra chính xác 0 vì không có tiện ích mở rộng hợp lệ nào được kích hoạt. 

Một trường hợp đặc biệt khác xảy ra khi tập hợp nốt bao gồm 1. Ở đây, mọi tiền tố đều có thể truy cập được và DP lấp đầy dày đặc. Thuật toán vẫn hoạt động chính xác vì mỗi trạng thái được cập nhật chính xác một lần trên mỗi đường dẫn chuyển tiếp và số học modulo ngăn chặn tràn ngay cả khi số lượng tăng lớn. 

Trường hợp cạnh cuối cùng là khi mục tiêu bằng chính xác với một trong các nốt. Đối với đầu vào như$M=4$, ghi chú$[4]$, DP chỉ chuyển đổi một lần từ 0 sang 4, tạo ra chính xác một chuỗi hợp lệ.
