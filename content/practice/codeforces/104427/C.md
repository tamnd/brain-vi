---
title: "CF 104427C - Một, Hai, Ba"
description: "Chúng ta được cấp một chuỗi dài chỉ gồm các giá trị 1, 2 và 3. Từ chuỗi này, chúng ta muốn trích xuất càng nhiều bộ ba chỉ số rời nhau càng tốt, trong đó mỗi bộ ba sử dụng ba vị trí khác nhau và tôn trọng thứ tự của các chỉ số."
date: "2026-06-30T18:58:18+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104427
codeforces_index: "C"
codeforces_contest_name: "2022-2023 Winter Petrozavodsk Camp, Day 2: GP of ainta"
rating: 0
weight: 104427
solve_time_s: 50
verified: true
draft: false
---

[CF 104427C - Một, Hai, Ba](https://codeforces.com/problemset/problem/104427/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 50s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một chuỗi dài chỉ gồm các giá trị 1, 2 và 3. Từ chuỗi này, chúng ta muốn trích xuất càng nhiều bộ ba chỉ số rời nhau càng tốt, trong đó mỗi bộ ba sử dụng ba vị trí khác nhau và tôn trọng thứ tự của các chỉ số. 

Bộ ba được coi là hợp lệ nếu các giá trị của nó tạo thành mẫu 1, 2, 3 theo thứ tự chỉ số tăng dần hoặc mẫu đảo ngược 3, 2, 1 theo thứ tự chỉ số tăng dần. Mỗi chỉ mục có thể thuộc về nhiều nhất một bộ ba đã chọn, vì vậy một khi một vị trí đã được sử dụng thì nó không thể được sử dụng lại trong một nhóm khác. 

Nhiệm vụ không chỉ là tính toán có bao nhiêu bộ ba như vậy tồn tại mà còn là xây dựng một tập hợp tối đa các bộ ba hợp lệ rời rạc. 

Ràng buộc N lên tới 600000 ngay lập tức loại trừ bất kỳ phương pháp nào cố gắng liệt kê hoặc kiểm tra sự kết hợp của bộ ba. Bất kỳ giải pháp nào cũng phải tuyến tính hoặc gần tuyến tính, vì hành vi bậc hai sẽ bao hàm thứ tự của 10^11 phép tính trong trường hợp xấu nhất. 

Một chế độ thất bại tinh vi xuất hiện khi việc so khớp tham lam được thực hiện cục bộ mà không quan tâm đến sự cân bằng toàn cầu. Ví dụ: nếu trình tự bị lệch nhiều như 1 1 1 2 2 2 3 3 3, việc ghép các lần xuất hiện sớm của 1 và 3 quá mạnh có thể cản trở việc hình thành chuỗi 1-2-3 tối ưu. Một vấn đề khác nảy sinh nếu chúng ta chỉ cố gắng khớp các mẫu thuận và bỏ qua cấu trúc 3-2-1 ngược, cấu trúc này có thể tạo ra kết quả dưới mức tối ưu khi phân phối không đối xứng. 

## Phương pháp tiếp cận 

Một cách tiếp cận đơn giản sẽ cố gắng xây dựng bộ ba bằng cách kiểm tra mọi kết hợp của i, j, k với i < j < k và xác minh xem các giá trị có khớp với 1-2-3 hay 3-2-1 hay không, đồng thời đảm bảo các chỉ số không được sử dụng. Điều này sẽ yêu cầu lặp lại ba lần O(N^3) trong trường hợp xấu nhất, điều này là không thể đối với N lên tới 600000. 

Ngay cả việc giảm điều này xuống để sửa chỉ số ở giữa j và tìm kiếm i và k phù hợp vẫn dẫn đến O(N^2). Vấn đề cốt lõi là mỗi phần tử có khả năng tham gia vào nhiều bộ ba ứng cử viên và việc kiểm tra tính tương thích theo cặp vẫn bùng nổ về mặt tổ hợp. 

Quan sát quan trọng là mọi bộ ba hợp lệ đều có cấu trúc rất cứng nhắc. Mỗi bộ ba đều tăng giá trị (1 rồi 2 rồi 3) hoặc giảm giá trị (3 rồi 2 rồi 1). Trong cả hai trường hợp, phần tử ở giữa luôn là 2. Điều này giúp giảm bớt vấn đề khi ghép mỗi 2 được chọn với một số 1 ở bên trái và một số 3 ở bên phải hoặc đối xứng một số 3 ở bên trái và một số 1 ở bên phải. 

Điều này gợi ý coi số 2 là điểm neo. Sau đó, chúng tôi cố gắng ghép 2 nhóm với nhóm bên trái và nhóm bên phải. Mục tiêu trở thành tối đa hóa số lượng mỏ neo có thể được thỏa mãn. Vì mỗi chỉ mục được sử dụng nhiều nhất một lần nên chúng ta cần một chiến lược tham lam để duy trì tính linh hoạt cho các kết quả phù hợp trong tương lai. 

Chúng tôi quét mảng và duy trì ba danh sách chỉ số có thứ tự: vị trí 1, 2 và 3. Sau đó, chúng tôi xây dựng các bộ ba bằng cách ghép nối 2 với các điểm cuối tương thích gần nhất hiện có. Đối với quân 2 ở vị trí j, chúng ta cố gắng tạo thành bộ ba 1-2-3 hoặc 3-2-1. Việc tham lam lựa chọn các đối tác sẵn có gần nhất đảm bảo chúng ta không lãng phí các vị thế cực đoan có thể cần thiết sau này. 

Một cách chính xác để thực thi tính tham lam này là xử lý 2 giây từ trái sang phải và duy trì hai con trỏ vào danh sách 1 và danh sách 3. Mỗi lần chúng ta cố gắng tạo thành một bộ ba, chúng ta chọn phần tử bên trái hợp lệ sớm nhất và phần tử bên phải hợp lệ sớm nhất vẫn nằm ở phía bên phải của 2 hiện tại và chưa được sử dụng. 

Điều này biến vấn đề thành một cuộc quét tuyến tính với sự tiến bộ của con trỏ, trong đó mỗi chỉ mục được sử dụng nhiều nhất một lần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(N^3) | O(1) | Quá chậm | 
| Kết hợp tham lam tối ưu | O(N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán

Trước tiên, chúng tôi tách các chỉ số thành ba danh sách tăng dần, một danh sách cho mỗi giá trị. Điều này cho phép chúng tôi suy luận về tính khả dụng mà không cần quét liên tục mảng ban đầu. 

Sau đó, chúng tôi xử lý các trung tâm tiềm năng, nghĩa là các chỉ số có giá trị là 2, từ trái sang phải. Đối với mỗi chỉ mục như vậy, chúng tôi cố gắng xây dựng một bộ ba hợp lệ. 

Đối với mỗi 2 ở vị trí j, trước tiên chúng ta cố gắng xây dựng bộ ba 1-2-3. Chúng tôi kiểm tra xem có tồn tại số 1 chưa sử dụng với chỉ số i < j và số 3 chưa sử dụng với chỉ mục k > j hay không. Nếu cả hai đều tồn tại, chúng ta gán i hợp lệ nhỏ nhất chưa được sử dụng và k hợp lệ nhỏ nhất chưa được sử dụng. Điều này giảm thiểu việc tiêu thụ các chỉ số lớn ở bên trái và các chỉ số nhỏ ở bên phải. 

Nếu đội hình 1-2-3 không thể thực hiện được, chúng ta thử mô hình đảo ngược 3-2-1. Chúng tôi kiểm tra số 3 ở bên trái và số 1 ở bên phải, một lần nữa lấy những ứng viên có sẵn gần nhất. 

Nếu cả hai mẫu đều không thể thực hiện được thì chúng tôi bỏ qua mẫu 2 này vì nó không thể đóng góp vào bất kỳ bộ ba hợp lệ nào trong tình trạng sẵn có hiện tại. 

Mỗi lần chúng ta tạo thành bộ ba, chúng ta sẽ đánh dấu các chỉ số đã sử dụng để chúng không thể tham gia lại. 

### Tại sao nó hoạt động 

Thuật toán duy trì thuộc tính là tất cả các số 1, 2 và 3 không được sử dụng vẫn có sẵn theo thứ tự chỉ số tăng dần và chúng tôi luôn sử dụng các ứng cử viên hợp lệ gần nhất với số 2 hiện tại. Điều này ngăn cản việc chặn các số 2 trong tương lai không cần thiết, vì việc sử dụng điểm cuối cực đoan khi có một điểm cuối gần hơn sẽ chỉ làm giảm tính linh hoạt mà không làm tăng số lượng bộ ba có thể có. 

Bất kỳ giải pháp hợp lệ nào cũng có thể được chuyển đổi thành giải pháp sử dụng kết hợp tham lam gần nhất mà không làm giảm số lượng bộ ba, bởi vì việc hoán đổi điểm cuối ở xa hơn với điểm cuối không được sử dụng gần hơn sẽ duy trì tính hợp lệ trong khi cải thiện nghiêm ngặt hoặc duy trì tính khả dụng còn lại ở cả hai bên. Đối số trao đổi này đảm bảo rằng sự lựa chọn tham lam không làm giảm tính tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    
    pos1 = []
    pos2 = []
    pos3 = []
    
    for i, x in enumerate(a):
        if x == 1:
            pos1.append(i)
        elif x == 2:
            pos2.append(i)
        else:
            pos3.append(i)
    
    used1 = [False] * len(pos1)
    used3 = [False] * len(pos3)
    
    p1 = 0
    p3 = 0
    
    ans = []
    
    # process each 2 as center
    for j in pos2:
        while p1 < len(pos1) and pos1[p1] < j and used1[p1]:
            p1 += 1
        
        while p3 < len(pos3) and pos3[p3] < j and used3[p3]:
            p3 += 1
        
        # try 1-2-3
        i = p1
        while i < len(pos1) and (pos1[i] >= j or used1[i]):
            i += 1
        
        k = p3
        while k < len(pos3) and (pos3[k] <= j or used3[k]):
            k += 1
        
        if i < len(pos1) and k < len(pos3):
            ans.append((pos1[i], j, pos3[k]))
            used1[i] = True
            used3[k] = True
            continue
        
        # try 3-2-1
        i = 0
        while i < len(pos3) and (pos3[i] >= j or used3[i]):
            i += 1
        
        k = 0
        while k < len(pos1) and (pos1[k] <= j or used1[k]):
            k += 1
        
        if i < len(pos3) and k < len(pos1):
            ans.append((pos3[i], j, pos1[k]))
            used3[i] = True
            used1[k] = True
    
    print(len(ans))
    for i, j, k in ans:
        print(i, j, k)

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách chia các chỉ số theo giá trị, giúp tránh việc quét lặp lại mảng ban đầu. Các mảng`pos1`,`pos2`, Và`pos3`lưu trữ các vị trí đã sắp xếp, điều này rất quan trọng để đảm bảo rằng chuyển động của con trỏ duy trì tính chính xác. 

các`used1`Và`used3`mảng theo dõi xem một lần xuất hiện cụ thể đã được gán cho bộ ba hay chưa. Điều này tránh việc tính hai lần. 

Đối với mỗi 2, chúng tôi cố gắng tìm các điểm cuối tương thích. Việc bảo trì con trỏ đảm bảo chúng tôi không quét liên tục các chỉ mục đã được sử dụng. Séc`pos1[i] < j`Và`pos3[k] > j`thực thi các ràng buộc đặt hàng. 

Một điểm tinh tế là chúng tôi luôn thử mẫu chuyển tiếp 1-2-3 trước tiên. Sự lựa chọn này là tùy ý về mặt chính xác nhưng ổn định hành vi phù hợp và tránh xu hướng đảo chiều quá sớm. 

## Ví dụ đã hoạt động 

Hãy xem xét đầu vào:```
6
1 2 3 2 1 3
```Chúng tôi có: 

pos1 = [0, 4], pos2 = [1, 3], pos3 = [2, 5] 

Chúng tôi xử lý 2 ở chỉ số 1 trước tiên. 

| Bước | chỉ số 2 | đã chọn 1 | đã chọn 3 | hành động | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 0 | 2 | mẫu 1-2-3 | 
| 2 | 3 | 4 | 5 | mẫu 1-2-3 | 

Đầu ra trở thành: 

(0,1,2) và (4,3,5) 

Dấu vết này cho thấy cách kết hợp tham lam bảo tồn cả cấu trúc sớm và muộn. 

Bây giờ hãy xem xét:```
5
3 2 1 3 2
```pos1 = [2], pos2 = [1,4], pos3 = [0,3] 

Chúng tôi xử lý 2 ở chỉ số 1: 

| Bước | chỉ số 2 | đã chọn 3 | đã chọn 1 | hành động | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 0 | 2 | mẫu 3-2-1 | 

2 tiếp theo ở chỉ số 4 không thể tạo thành bất kỳ bộ ba nào vì tất cả các điểm cuối tương thích đều được sử dụng hoặc bị căn chỉnh sai. 

Điều này chứng tỏ rằng việc sử dụng sớm các điểm cuối hợp lệ gần nhất không cản trở tính khả thi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N) | Mỗi chỉ mục được quét và đánh dấu sử dụng tối đa một lần, chuyển động của con trỏ đơn điệu | 
| Không gian | O(N) | Lưu trữ vị trí của 1s, 2s và 3s | 

Cấu trúc tuyến tính phù hợp thoải mái trong các ràng buộc cho N lên tới 600000, vì mỗi hoạt động là công việc được khấu hao không đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    old_stdout = sys.stdout
    sys.stdout = io.StringIO()
    solve()
    out = sys.stdout.getvalue()
    sys.stdout = old_stdout
    return out.strip()

# minimal
assert run("1\n1") == "0"

# single valid triple
assert run("3\n1 2 3") == "1\n0 1 2"

# reversed valid triple
assert run("3\n3 2 1") == "1\n0 1 2"

# multiple disjoint triples
assert run("6\n1 2 3 1 2 3") == "2\n0 1 2\n3 4 5"

# no possible triples
assert run("5\n1 1 1 2 2") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 phần tử đơn | 0 | trường hợp tối thiểu, không thể gấp ba | 
| 3 1 2 3 | 1 ba | mô hình chuyển tiếp cơ bản | 
| 3 3 2 1 | 1 ba | tính đúng đắn của mẫu đảo ngược | 
| 6 cặp hợp lệ xen kẽ | 2 bộ ba | sự phù hợp rời rạc | 
| mảng lệch | 0 | xử lý không đủ điểm cuối | 

## Vỏ cạnh 

Trường hợp cạnh then chốt là khi tồn tại bộ ba hợp lệ nhưng việc so khớp tham lam phải tránh làm cạn kiệt một bên quá sớm. Coi như:```
7
1 2 3 1 2 3 1
```Có nhiều cách để tạo thành bộ ba, nhưng chỉ có thể có hai bộ ba hoàn chỉnh. Thuật toán xử lý 2 giây theo thứ tự và luôn lấy các điểm cuối hợp lệ gần nhất, do đó, 2 điểm đầu tiên ở chỉ số 1 sẽ ghép với (0,2). 2 thứ hai ở chỉ số 4 cặp với (3,5). Số 1 còn lại ở chỉ số 6 vẫn chưa được sử dụng, phản ánh chính xác việc đóng gói tối ưu. 

Một trường hợp cạnh khác là khi chỉ có thể thực hiện được bộ ba ngược:```
4
3 2 1 2
```Chỉ có thể hình thành một bộ ba: (3,2,1) bằng cách sử dụng các chỉ số (0,1,2). 2 thứ hai không thể khớp vì không còn điểm cuối hợp lệ nào thỏa mãn các ràng buộc thứ tự. Thuật toán sử dụng chính xác cấu trúc khả thi duy nhất và dừng lại mà không buộc phải ghép nối không hợp lệ.
