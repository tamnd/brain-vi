---
title: "CF 104180C - Làm Bánh Brownie"
description: "Chúng ta được cho hai tập hợp số nguyên. Một cái tượng trưng cho những kích cỡ bánh hạnh nhân cần thiết mà bạn bè yêu cầu, và cái còn lại tượng trưng cho những hộp nướng bánh có sẵn, trong đó mỗi hộp thiếc sẽ tạo ra chính xác một chiếc bánh hạnh nhân có kích thước cố định riêng."
date: "2026-07-02T00:42:49+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104180
codeforces_index: "C"
codeforces_contest_name: "UTPC Contest 02-10-23 Div. 2 (Beginner)"
rating: 0
weight: 104180
solve_time_s: 105
verified: true
draft: false
---

[CF 104180C - Nướng bánh hạnh nhân](https://codeforces.com/problemset/problem/104180/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 45s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho hai tập hợp số nguyên. Một cái tượng trưng cho những kích cỡ bánh hạnh nhân cần thiết mà bạn bè yêu cầu, và cái còn lại tượng trưng cho những hộp nướng bánh có sẵn, trong đó mỗi hộp thiếc sẽ tạo ra chính xác một chiếc bánh hạnh nhân có kích thước cố định riêng. Một người bạn hài lòng nếu chiếc bánh hạnh nhân được giao cho họ có kích thước lớn hơn những gì họ yêu cầu. Mỗi hộp thiếc có thể được sử dụng tối đa một lần và mỗi người bạn có thể nhận được tối đa một hộp thiếc. 

Nhiệm vụ là ghép các hộp thiếc với bạn bè sao cho tối đa hóa số lượng bạn bè hài lòng. 

Cấu trúc chính ở đây là vấn đề so khớp một-một giữa hai mảng, với ràng buộc bất bình đẳng nghiêm ngặt về tính khả thi và mục tiêu chung là tối đa hóa số lượng kết quả khớp thành công. 

Các ràng buộc lên tới 200.000 phần tử trong mỗi mảng. Một giải pháp bậc hai thử tất cả các cặp hoặc kiểm tra tham lam từng người bạn với tất cả các hộp thiếc sẽ thực hiện theo thứ tự 40 tỷ so sánh trong trường hợp xấu nhất, vượt xa giới hạn 2 giây. Điều này ngay lập tức gợi ý rằng bất kỳ giải pháp khả thi nào cũng phải có nhiều nhất là O(n log n) hoặc O(n). 

Một số trường hợp đặc biệt quan trọng: 

Nếu tất cả các hộp thiếc quá nhỏ, ví dụ như yêu cầu của bạn bè`[10, 20, 30]`và hộp thiếc là`[1, 2, 3]`, câu trả lời là bằng không. Bất kỳ kẻ tham lam nào ghép cặp từ nhỏ nhất đến nhỏ nhất mà không kiểm tra bất đẳng thức nghiêm ngặt vẫn sẽ thất bại ở đây, nhưng lỗi bất đẳng thức đảo ngược thường gây ra việc đếm không chính xác. 

Nếu hộp thiếc lớn hơn đáng kể so với yêu cầu, chẳng hạn như yêu cầu`[1, 1, 1]`và hộp thiếc`[100, 101, 102]`, mọi người bạn nên hài lòng. Một giải pháp có lỗi có thể lãng phí những hộp thiếc lớn cho những yêu cầu nhỏ đã có thể đáp ứng được theo thứ tự không tối ưu nếu nó không phối hợp khớp một cách cẩn thận. 

Một trường hợp thất bại tinh vi phát sinh khi tồn tại các bản sao, ví dụ như các yêu cầu`[5, 5, 5]`và hộp thiếc`[6, 6]`. Câu trả lời đúng là 2. Một cách tiếp cận ngây thơ luôn ghép hộp thiếc tương thích đầu tiên mà nó nhìn thấy có thể vô tình sử dụng lại logic không đánh dấu chính xác hộp thiếc là đã tiêu thụ, dẫn đến việc đếm quá mức. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là thử gán hộp thiếc cho bạn bè theo cách đệ quy hoặc thông qua đối sánh hai bên. Chúng ta có thể thử ghép nối giữa một người bạn và một hộp thiếc chưa sử dụng, chọn chỉ định hay bỏ qua. Điều này tạo thành một không gian tìm kiếm phù hợp hai bên với sự phân nhánh ở mọi lựa chọn. Ngay cả khi chúng tôi loại bỏ các kết quả khớp không hợp lệ, trong trường hợp xấu nhất khi hầu hết các hộp thiếc đều đủ lớn, số lượng kết quả khớp một phần hợp lệ sẽ tăng lên theo kiểu tổ hợp. Với 200.000 phần tử thì điều này hoàn toàn không khả thi. 

Một quan điểm có cấu trúc hơn là sắp xếp cả hai mảng. Sau khi được sắp xếp, chúng ta có thể cố gắng luôn làm hài lòng người bạn ít đòi hỏi nhất trước tiên bằng cách sử dụng hộp thiếc nhỏ nhất có thể làm hài lòng họ. Đây là nhận xét quan trọng: nếu chúng ta chỉ định một hộp thiếc lớn cho một yêu cầu nhỏ trong khi hộp thiếc nhỏ hơn có thể đáp ứng được, thì chúng ta có thể giảm tính linh hoạt trong tương lai mà không mang lại lợi ích gì. Chiến lược đúng đắn là giảm thiểu lãng phí công suất bằng cách luôn sử dụng hộp thiếc nhỏ nhất có thể vẫn còn dùng được. 

Điều này dẫn đến quá trình so khớp tham lam trên các mảng đã được sắp xếp. Chúng tôi quét các yêu cầu theo thứ tự tăng dần và duy trì con trỏ trên các hộp thiếc, tiến tới con trỏ đó cho đến khi chúng tôi tìm thấy hộp thiếc vượt quá yêu cầu hiện tại. Khi tìm thấy, chúng tôi khớp và di chuyển cả hai con trỏ về phía trước. Nếu không, chúng tôi dừng yêu cầu đó và tiếp tục. 

Điều này làm giảm vấn đề thành việc quét hai con trỏ trên các mảng đã được sắp xếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
|---|---|---|---| 
| Lực lượng vũ phu | O(N·M) hoặc hàm mũ | O(N + M) | Quá chậm | 
| Tham lam tối ưu + Sắp xếp | O(N log N + M log M) | O(1) bổ sung (bỏ qua sắp xếp) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp cả mảng yêu cầu và mảng tin theo thứ tự không giảm. Điều này đảm bảo chúng tôi luôn xử lý các yêu cầu dễ dàng hơn trước những yêu cầu khó hơn, giúp tránh lãng phí hộp thiếc lớn cho các yêu cầu nhỏ. 

2. Khởi tạo hai con trỏ: một cho yêu cầu, một cho hộp thiếc, cả hai đều bắt đầu từ chỉ số 0. Cũng duy trì một bộ đếm cho các trận đấu thành công. 

3. Đối với yêu cầu hiện tại, hãy di chuyển con trỏ thiếc về phía trước cho đến khi chúng tôi tìm thấy hộp thiếc đầu tiên có kích thước lớn hơn yêu cầu. Mỗi tin bị bỏ qua quá nhỏ để đáp ứng yêu cầu này hoặc bất kỳ yêu cầu nào trước đó vì các yêu cầu đã được sắp xếp. 

4. Nếu tìm thấy hộp thiếc như vậy, hãy gán nó cho yêu cầu hiện tại, tăng bộ đếm khớp và nâng cao cả hai con trỏ. Điều này đảm bảo mỗi hộp thiếc được sử dụng nhiều nhất một lần. 

5. Nếu không có tin nào phù hợp, chỉ di chuyển con trỏ yêu cầu về phía trước. Yêu cầu này không thể được đáp ứng bởi bất kỳ thiếc nào còn lại. 

6. Tiếp tục cho đến khi hết một trong hai danh sách. 

Chi tiết quan trọng là con trỏ thiếc không bao giờ di chuyển lùi. Khi hộp thiếc quá nhỏ so với một yêu cầu nhất định, nó cũng sẽ quá nhỏ đối với bất kỳ yêu cầu nào sau này vì yêu cầu chỉ tăng lên. 

### Tại sao nó hoạt động 

Tính chính xác phụ thuộc vào cấu trúc khả thi đơn điệu sau khi sắp xếp. Khi cả hai mảng được sắp xếp, việc khớp hộp tin khả thi nhỏ nhất với yêu cầu nhỏ nhất chưa được đáp ứng luôn an toàn vì bất kỳ nhiệm vụ thay thế nào sử dụng hộp tin lớn hơn cho yêu cầu này đều không thể cải thiện số lượng kết quả khớp sau này. Hộp thiếc lớn linh hoạt hơn nên việc bảo quản chúng cho những yêu cầu lớn hơn không thể làm giảm tính tối ưu. Lập luận trao đổi này đảm bảo rằng bất kỳ giải pháp tối ưu nào cũng có thể được chuyển đổi thành một giải pháp theo sau sự kết hợp tham lam này mà không làm giảm số lượng bạn bè hài lòng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    req = list(map(int, input().split()))
    tins = list(map(int, input().split()))
    
    req.sort()
    tins.sort()
    
    i = j = 0
    ans = 0
    
    while i < n and j < m:
        while j < m and tins[j] <= req[i]:
            j += 1
        if j < m:
            ans += 1
            i += 1
            j += 1
        else:
            break
    
    print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách sắp xếp cả hai mảng để chúng ta có thể xử lý an toàn từ nhỏ nhất đến lớn nhất. Hai con trỏ`i`Và`j`theo dõi vị trí của chúng tôi trong các yêu cầu và hộp thiếc tương ứng. 

Vòng lặp bên trong tiến lên`j`cho đến khi chúng ta tìm thấy một hộp thiếc lớn hơn`req[i]`hoặc xả hết hộp thiếc. Sự bất bình đẳng nghiêm ngặt là quan trọng; sử dụng`<=`đảm bảo chúng tôi không khớp sai các hộp thiếc có kích thước bằng nhau. 

Sau khi tìm thấy một hộp thiếc hợp lệ, chúng tôi coi đó là một nhiệm vụ thành công và chuyển tiếp cả hai con trỏ, tiêu thụ cả yêu cầu và hộp thiếc. 

Nếu không có tin nào có thể đáp ứng yêu cầu hiện tại, vòng lặp kết thúc và câu trả lời được in ra. 

Một lỗi triển khai phổ biến là quên nâng cao con trỏ yêu cầu khi xảy ra kết quả trùng khớp, điều này sẽ sử dụng lại cùng một yêu cầu nhiều lần một cách không chính xác. Một cách khác là sử dụng một lượt mà không phân loại, điều này phá vỡ cấu trúc tham lam. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
5 3
8 12 25 3 10
1 8 20
```Đã sắp xếp: 
Yêu cầu = [3, 8, 10, 12, 25] 
Hộp thiếc = [1, 8, 20] 

| tôi (yêu cầu) | j (thiếc) | yêu cầu[i] | hộp thiếc[j] | Hành động | Trận đấu | 
|---|---|---|---|---|---| 
| 0 | 0 | 3 | 1 | 1 ≤ 3, bỏ qua tin | 0 | 
| 0 | 1 | 3 | 8 | 8 > 3, khớp | 1 | 
| 1 | 2 | 8 | 20 | 20 > 8, trận đấu | 2 | 
| 2 | 3 | 10 | - | không còn hộp thiếc | 2 | 

Đầu ra là 2. 

Dấu vết này cho thấy các hộp thiếc nhỏ được loại bỏ một cách chính xác khi chúng không thể đáp ứng yêu cầu nhỏ nhất và các hộp thiếc lớn hơn được bảo quản cho các yêu cầu lớn hơn. 

### Mẫu 2 (đã thi công) 

đầu vào:```
4 4
5 5 5 5
6 4 6 7
```Đã sắp xếp: 
Yêu cầu = [5, 5, 5, 5] 
Hộp thiếc = [4, 6, 6, 7] 

| tôi | j | yêu cầu[i] | hộp thiếc[j] | Hành động | Trận đấu | 
|---|---|---|---|---|---| 
| 0 | 0 | 5 | 4 | quá nhỏ | 0 | 
| 0 | 1 | 5 | 6 | trận đấu | 1 | 
| 1 | 2 | 5 | 6 | trận đấu | 2 | 
| 2 | 3 | 5 | 7 | trận đấu | 3 | 
| 3 | 4 | - | - | xong | 3 | 

Điều này chứng tỏ rằng các bản sao được xử lý một cách tự nhiên và mỗi hộp thiếc được tiêu thụ đúng một lần. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
|---|---|---| 
| Thời gian | O(N log N + M log M) | sắp xếp chiếm ưu thế, quét hai con trỏ là tuyến tính | 
| Không gian | O(1) thêm | sắp xếp tại chỗ ngoài mảng đầu vào | 

Các ràng buộc cho phép tối đa 200.000 phần tử, do đó, việc sắp xếp ở quy mô này nằm trong giới hạn thoải mái và quá trình quét tuyến tính sau đó đảm bảo giải pháp vẫn hiệu quả. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# sample 1
assert run("""5 3
8 12 25 3 10
1 8 20
""") == "2"

# all match
assert run("""3 3
1 1 1
2 2 2
""") == "3"

# none match
assert run("""3 3
10 20 30
1 2 3
""") == "0"

# duplicates tin heavy
assert run("""3 5
5 5 5
6 6 6 6 6
""") == "3"

# exact boundary equality should fail equality
assert run("""2 2
5 5
5 6
""") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
|---|---|---| 
| tất cả các yêu cầu nhỏ, hộp thiếc lớn | 3 | có thể kết hợp đầy đủ | 
| tất cả các yêu cầu lớn, hộp thiếc nhỏ | 0 | không có trận đấu | 
| trùng lặp ở cả hai bên | 3 | tính chính xác với các giá trị lặp lại | 
| trường hợp ranh giới đẳng thức | 1 | xử lý bất bình đẳng nghiêm ngặt | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi nhiều hộp thiếc quá nhỏ đối với các yêu cầu ban đầu nhưng vẫn có thể đáp ứng các yêu cầu sau nếu chúng ta bỏ qua đúng cách. Ví dụ, các yêu cầu`[5, 6]`và hộp thiếc`[4, 7]`. Thuật toán đầu tiên loại bỏ`4`cho yêu cầu`5`, sau đó sử dụng đúng`7`cho yêu cầu`6`, đạt được một trận đấu. Một chiến lược tham lam cố gắng khớp từng yêu cầu một cách độc lập mà không đưa ra con trỏ thiếc một cách chính xác sẽ được sử dụng lại`4`hoặc kết luận sai sự cố. 

Một trường hợp khác là khi hộp thiếc có nhiều nhưng chỉ một số ít đủ lớn. Đối với yêu cầu`[1, 2, 3, 4]`và hộp thiếc`[2, 2, 2, 10]`, thuật toán đảm bảo rằng hộp thiếc lớn duy nhất được dành riêng cho yêu cầu lớn nhất có thể sử dụng nó, trong khi hộp thiếc nhỏ hơn được tiêu thụ bởi các yêu cầu nhỏ hơn, mang lại mức sử dụng tối ưu.
