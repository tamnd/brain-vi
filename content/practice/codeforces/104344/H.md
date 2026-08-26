---
title: "CF 104344H - Shrek II"
description: "Chúng ta được đưa cho hai đống đồng xu, một đống có đồng xu $A$ và một đống có đồng xu $B$. Hai người chơi luân phiên nhau và trong mỗi lượt, người chơi phải loại bỏ chính xác một đồng xu từ cọc đầu tiên hoặc từ cọc thứ hai hoặc từ cả hai cọc cùng một lúc."
date: "2026-07-01T18:30:05+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104344
codeforces_index: "H"
codeforces_contest_name: "Maratona dos Bixes 2023 - UNICAMP"
rating: 0
weight: 104344
solve_time_s: 95
verified: true
draft: false
---

[CF 104344H - Shrek II](https://codeforces.com/problemset/problem/104344/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 35s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được đưa cho hai đống tiền xu, một đống có$A$tiền xu và một cái khác với$B$tiền xu. Hai người chơi luân phiên nhau và trong mỗi lượt, người chơi phải loại bỏ chính xác một đồng xu từ cọc đầu tiên hoặc từ cọc thứ hai hoặc từ cả hai cọc cùng một lúc. Một động thái loại bỏ tiền xu là bắt buộc, vì vậy không được phép vượt qua. Người chơi loại bỏ đồng xu cuối cùng còn lại khỏi hệ thống sẽ thắng trò chơi. 

Chúng tôi được yêu cầu phân tích trò chơi này từ góc nhìn của người chơi đầu tiên, Burro. Đối với một cấu hình bắt đầu nhất định$(A, B)$, chúng ta phải quyết định xem người chơi đầu tiên có thể giành chiến thắng hay không nếu cả hai người chơi đều chơi tối ưu. Nếu chiến lược chiến thắng tồn tại, chúng ta cũng phải đưa ra nước đi đầu tiên hợp lệ để đảm bảo kết quả đó. 

Những ràng buộc cho phép$A, B \le 10^9$, điều này ngay lập tức cho chúng ta biết rằng bất kỳ sự khám phá không gian trạng thái nào trên tất cả các vị trí đều là không thể. Ngay cả việc quét tuyến tính trên tất cả các trạng thái cũng không thể thực hiện được và bất kỳ giải pháp nào cũng phải giảm trò chơi về điều kiện dạng đóng có thể tính toán được trong thời gian không đổi cho mỗi trường hợp thử nghiệm. 

Trường hợp có biên khó nhận thấy là trường hợp cả hai cọc đều trống. Trong trường hợp đó, trò chơi đã kết thúc và người chơi đầu tiên không có nước đi nào, điều này sẽ được xử lý theo tình trạng thua cuộc. 

Một trường hợp tế nhị khác là khi một đống trống rỗng. Tập nước đi vẫn cho phép lấy từ đống không trống hoặc lấy từ cả hai, nhưng hành vi trở nên không đối xứng và có thể đánh lừa lý luận chẵn lẻ ngây thơ nếu không được rút ra một cách cẩn thận từ cấu trúc trò chơi đầy đủ thay vì trực giác về các đống đơn lẻ. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ xử lý mọi vị trí$(a, b)$như một trạng thái trò chơi và thử đệ quy tất cả các bước di chuyển có thể: loại bỏ khỏi ngăn xếp đầu tiên, loại bỏ khỏi ngăn xếp thứ hai hoặc loại bỏ khỏi cả hai. Một thế cờ sẽ thắng nếu có ít nhất một nước đi dẫn đến thế thua và sẽ thua nếu tất cả các nước đi đều dẫn đến thế thắng. 

Phép đệ quy này mô hình hóa chính xác trò chơi, nhưng không gian trạng thái$(A+1)(B+1)$, trong trường hợp xấu nhất là theo thứ tự$10^{18}$. Ngay cả với tính năng ghi nhớ, số lượng trạng thái có thể truy cập vẫn quá lớn đối với bất kỳ chương trình lập trình động trực tiếp nào. 

Quan sát quan trọng là nước đi luôn giảm tổng số xu đúng một hoặc hai và trò chơi hoạt động giống như một trò chơi trừ có cấu trúc rất chặt chẽ trên một lưới. Việc tính toán các giá trị nhỏ cho thấy mô hình ổn định của các vị trí thua: bất cứ khi nào cả hai cọc đều chẵn, vị trí sẽ thua và tất cả các cấu hình khác đều thắng. 

Điều này làm cho toàn bộ trò chơi rơi vào tình trạng chẵn lẻ chứ không phải là vấn đề truyền tải đồ thị. Sau khi xác định được trạng thái thua, nước đi thắng có thể được thực hiện bằng cách buộc đối thủ vào trạng thái cả hai cọc đều chẵn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (cây trò chơi DP) |$O(AB)$|$O(AB)$| Quá chậm | 
| Đặc tính chẵn lẻ |$O(1)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

### Bước 1: Xác định vị thế thua lỗ 

Một vị thế sẽ thua nếu cả hai$A$Và$B$đều chẵn. Nếu trò chơi bắt đầu ở một vị trí như vậy, bất kỳ động thái nào nhất thiết phải phá vỡ thuộc tính này và mang lại cho đối thủ một cấu hình chiến thắng. 

### Bước 2: Nếu trạng thái hiện tại đang thua 

Nếu cả hai$A$Và$B$chẵn, kết quả là không có chiến lược chiến thắng nào dành cho người chơi đầu tiên. 

### Bước 3: Xây dựng nước đi thắng nếu ngược lại 

Chúng ta cố gắng di chuyển vào thế thua, tức là vào trạng thái mà cả hai cọc trở nên ngang nhau. 

### Bước 4: Chọn nước đi dựa trên tính chẵn lẻ 

Nếu$A$là số chẵn và$B$kỳ quặc, giảm đi$B$bằng 1. 

Nếu$A$thật kỳ quặc và$B$chẵn, giảm$A$bằng 1. 

Nếu cả hai đều là số lẻ, hãy lấy đồng thời một đồng xu ra khỏi mỗi cọc. 

Mỗi bước di chuyển này sẽ đảo ngược tính chẵn lẻ một cách chính xác theo cách cần thiết để đạt đến trạng thái chẵn. 

### Tại sao nó hoạt động 

Bất biến quan trọng là các vị trí chẵn chính xác là vị trí P của trò chơi. Từ bất kỳ trạng thái chẵn-chẵn nào, mỗi nước đi hợp lệ sẽ thay đổi ít nhất một điểm chẵn lẻ, tạo ra trạng thái không chẵn. Từ bất kỳ trạng thái không chẵn nào, luôn có một nước đi lật ngược số lẻ để đạt số chẵn trong một bước, bởi vì chúng ta có thể điều chỉnh tính chẵn lẻ một cách độc lập bằng cách sử dụng ba loại nước đi được phép. Điều này đảm bảo rằng trò chơi giảm bớt sự chuyển đổi bắt buộc xen kẽ giữa các khu vực thắng và thua. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

A, B = map(int, input().split())

# losing position for first player
if A % 2 == 0 and B % 2 == 0:
    print("N")
else:
    print("S")
    if A % 2 == 0 and B % 2 == 1:
        print("B")
    elif A % 2 == 1 and B % 2 == 0:
        print("A")
    else:
        print("A B")
```Mã trực tiếp mã hóa đặc tính chẵn lẻ. Điều kiện đầu tiên kiểm tra xem trạng thái bắt đầu có bị mất hay không, trong trường hợp đó không có nước đi nào được in ra. Nếu không, nó sẽ chọn một nước đi buộc cả hai cọc vào một cấu hình đồng đều. 

Cấu trúc quyết định đầy đủ đối với các trường hợp chẵn lẻ và mỗi nhánh tương ứng chính xác với một nước đi làm giảm trạng thái thành lớp thua cuộc đối với đối thủ. 

## Ví dụ đã hoạt động 

### Ví dụ 1: Đầu vào`1 1`| Bước | A | B | Hành động | Trạng thái kết quả | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | 1 | 1 | Kiểm tra tính chẵn lẻ | vừa kỳ quặc | 
| Di chuyển lựa chọn | 1 | 1 | xóa cả hai | (0, 0) | 

Điều này khẳng định khi cả hai cọc đều lẻ thì nước đi tối ưu là loại bỏ khỏi cả hai cọc, ngay lập tức buộc đối phương phải mất vị trí cuối cùng. 

Dấu vết cho thấy trò chơi chuyển trực tiếp sang trạng thái mất cơ sở như thế nào. 

### Ví dụ 2: Nhập liệu`2 1`| Bước | A | B | Hành động | Trạng thái kết quả | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | 2 | 1 | Kiểm tra tính chẵn lẻ | hỗn hợp | 
| Di chuyển lựa chọn | 2 | 1 | xóa khỏi B | (2, 0) | 

Sau khi đi nước đi, cả hai cọc đều chẵn, nghĩa là đối phương nhận thế thua. Điều này xác nhận rằng các trạng thái chẵn lẻ hỗn hợp luôn cho phép chuyển đổi trực tiếp sang vùng thua cuộc. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(1)$| Chỉ kiểm tra tính chẵn lẻ và một quyết định | 
| Không gian |$O(1)$| Không sử dụng cấu trúc dữ liệu bổ sung | 

Giải pháp này dễ dàng phù hợp trong giới hạn vì mỗi trường hợp thử nghiệm được giải quyết bằng một số phép tính số học không đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    A, B = map(int, input().split())
    if A % 2 == 0 and B % 2 == 0:
        return "N"
    if A % 2 == 0 and B % 2 == 1:
        return "S\nB"
    if A % 2 == 1 and B % 2 == 0:
        return "S\nA"
    return "S\nA B"

# provided samples
assert run("1 0") == "S\nA", "sample 1"
assert run("1 1") == "S\nA B", "sample 2"
assert run("2 1") == "S\nB", "sample 3"

# custom cases
assert run("0 0") == "N", "empty game"
assert run("2 2") == "N", "even-even losing state"
assert run("3 3") == "S\nA B", "odd symmetric win"
assert run("4 2") == "N", "even-even larger losing state"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 0 0 | N | trạng thái mất thiết bị đầu cuối | 
| 2 2 | N | quy tắc chẵn-chẵn | 
| 3 3 | S A B | nước đi thắng đối xứng lẻ | 
| 4 2 | N | trường hợp thua chẵn không rõ ràng | 

## Vỏ cạnh 

###Trường hợp: cả 2 cọc trống`(0, 0)`Thuật toán phân loại cái này thành chẵn-chẵn, do đó nó xuất ra`N`. Điều này phù hợp với thực tế là người chơi di chuyển không có động thái hợp pháp và do đó sẽ thua ngay lập tức. 

### Trường hợp: trạng thái chẵn-chẵn lớn`(10^9, 10^9)`Cả hai giá trị đều là số chẵn nên kết quả đầu ra là`N`trong thời gian không đổi. Không cần thăm dò các chuyển đổi trạng thái vì chỉ tính chẵn lẻ mới quyết định kết quả. 

### Trường hợp: chẵn lẻ hỗn hợp`(even, odd)`Thuật toán luôn giảm đống lẻ đi một, tạo ra trạng thái chẵn-chẵn. Ví dụ,`(6, 5)`trở thành`(6, 4)`, buộc đối thủ rơi vào thế thua. 

### Trường hợp: cả hai đều lẻ`(odd, odd)`việc di chuyển`(A-1, B-1)`luôn luôn hợp lệ và chuyển đổi vị trí thành chẵn-chẵn. Ví dụ,`(7, 3)`trở thành`(6, 2)`, lại giao thế thua cho đối thủ.
