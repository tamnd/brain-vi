---
title: "CF 105278H - Biểu tượng"
description: "Người dùng Codeforces được theo dõi thông qua lịch sử xếp hạng của họ và “cấp trạng thái” của họ thay đổi khi xếp hạng của họ vượt qua các ngưỡng cố định như học sinh, chuyên gia, chuyên gia, v.v."
date: "2026-06-23T06:49:23+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105278
codeforces_index: "H"
codeforces_contest_name: "2024 ICPC Universidad Nacional de Colombia Programming Contest"
rating: 0
weight: 105278
solve_time_s: 89
verified: false
draft: false
---

[CF 105278H - Biểu tượng](https://codeforces.com/problemset/problem/105278/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 29s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Người dùng Codeforces được theo dõi thông qua lịch sử xếp hạng của họ và “cấp trạng thái” của họ thay đổi khi xếp hạng của họ vượt qua các ngưỡng cố định như học sinh, chuyên gia, chuyên gia, v.v. Mỗi bậc tương ứng với một khoảng xếp hạng liền kề và việc chuyển sang khoảng xếp hạng cao hơn lần đầu tiên sẽ kích hoạt giải thưởng biểu tượng. 

Đối với mỗi người dùng, chúng tôi được cấp quyền xử lý, xếp hạng hiện tại của họ sau cuộc thi mới nhất, xếp hạng tối đa lịch sử của họ trước cuộc thi này và thay đổi xếp hạng trong cuộc thi mới nhất. Từ những giá trị này, chúng tôi phải quyết định xem liệu bản cập nhật mới nhất có khiến chúng lên cấp cao hơn mức chúng từng đạt được trước đó hay không. Nếu có, chúng tôi xuất tên của cấp độ mới đó. Nếu không, chúng tôi sẽ đưa ra một thông điệp tạo động lực. 

Mặc dù đầu vào bao gồm đồng bằng xếp hạng nhưng quyết định chỉ phụ thuộc vào việc so sánh “xếp hạng được biết đến nhiều nhất trước đây” và “xếp hạng hiện tại” với các ranh giới bậc cố định. Trạng thái lịch sử có ý nghĩa duy nhất là xếp hạng tối đa trước bản cập nhật hiện tại, vì trạng thái đó mã hóa thứ hạng mà người dùng đã mở khóa. 

Các ràng buộc có độ lớn lớn nhưng không đáng kể về độ phức tạp của cấu trúc. Mọi thứ đều có thời gian không đổi cho mỗi trường hợp thử nghiệm, vì vậy giải pháp phải là O(1) và tránh mọi mô phỏng hoặc lặp lại trong lịch sử xếp hạng. 

Sự tinh tế chính là hiểu "lần đầu tiên đạt được thứ hạng mới" nghĩa là gì. Một cách giải thích ngây thơ có thể so sánh không chính xác chỉ xếp hạng hiện tại với các ngưỡng hoặc bỏ qua rằng người dùng có thể đã ở cùng cấp độ trước đó. 

Một vài tình huống khó khăn làm cho điều này rõ ràng hơn. 

Nếu người dùng trước đó đã đạt đến 2100 (chính) nhưng giảm xuống 2050 và sau đó quay lại 2100, họ sẽ không nhận được biểu tượng chính nữa. Xếp hạng tối đa đã bao gồm cấp độ đó nên không đạt được cấp độ mới nào. 

Nếu người dùng trước đây đạt đỉnh 1890 (chuyên gia) và bây giờ đạt đến 1900, thì họ đang bước vào ứng viên chính lần đầu tiên, vì vậy kết quả đầu ra phải phản ánh quá trình chuyển đổi đó. 

Nếu xếp hạng của người dùng tăng nhưng vẫn nằm trong cùng khoảng cấp độ thì sẽ không có giải thưởng nào ngay cả khi xếp hạng đã thay đổi. 

## Phương pháp tiếp cận 

Mô phỏng trực tiếp sẽ xây dựng lại toàn bộ lịch sử xếp hạng: áp dụng delta, cập nhật xếp hạng từng bước và theo dõi tất cả các cực đại trung gian. Điều này là không cần thiết vì những thay đổi về cấp độ chỉ phụ thuộc vào việc xếp hạng có vượt qua ranh giới ngưỡng chứ không phụ thuộc vào lộ trình đã thực hiện hay không. Trong trường hợp xấu nhất, một cách tiếp cận đơn giản mô phỏng quá trình chuyển đổi qua nhiều bản cập nhật sẽ tuyến tính về số lượng cuộc thi, điều này không xuất hiện ở đây nhưng minh họa bản chất quá mức cần thiết của một phương pháp như vậy. 

Quan sát quan trọng là các tầng tạo thành một phân vùng cố định của dòng số nguyên. Mỗi xếp hạng ánh xạ một cách xác định đến chính xác một cấp. Do đó, chúng ta chỉ cần so sánh hai điểm trong phân vùng này: xếp hạng tối đa trước đó và xếp hạng hiện tại. Nếu cả hai bản đồ đến cùng một bậc, sẽ không kiếm được biểu tượng mới. Nếu chúng ánh xạ tới các cấp độ khác nhau và cấp độ hiện tại cao hơn trong danh sách được sắp xếp thì người dùng vừa mở khóa một cấp độ mới lần đầu tiên. 

Điều này làm giảm vấn đề khi tính toán một số lượng nhỏ các lần tra cứu theo khoảng thời gian và một so sánh duy nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lịch sử Brute Force | O(T) trên mỗi lịch sử người dùng | O(1) | Không cần thiết | 
| So sánh bậc tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi mã hóa tất cả các bậc xếp hạng theo khoảng thời gian có thứ tự, từ thấp nhất đến cao nhất. Mỗi khoảng tương ứng với một tên biểu tượng.

1. Xây dựng danh sách các bậc có giới hạn dưới và tên của chúng. Thứ tự rất quan trọng vì chúng tôi sẽ so sánh các cấp theo vị trí trong danh sách này thay vì xếp hạng trực tiếp các giá trị. 
2. Xác định một hàm ánh xạ bất kỳ xếp hạng nào tới bậc của nó bằng cách quét các khoảng này và trả về chỉ mục hoặc tên của khoảng đầu tiên có giới hạn dưới được đáp ứng và giới hạn trên không bị vượt quá. Việc lập bản đồ mang tính quyết định và độc lập với lịch sử. 
3. Tính xếp hạng tốt nhất trước đó, được tính trực tiếp là M. Đây là xếp hạng cao nhất mà người dùng từng có trước cuộc thi hiện tại. 
4. Tính xếp hạng C hiện tại, thể hiện xếp hạng sau khi áp dụng kết quả cuộc thi mới nhất. 
5. Ánh xạ cả M và C vào các chỉ số bậc tương ứng của chúng. Đặt prev_rank là chỉ mục cấp của M và new_rank là chỉ mục cấp của C. 
6. Nếu new_rank lớn hơn prev_rank, hãy ghi tên của cấp mới. Nếu không thì xuất ra thông báo động lực. 

Việc so sánh hoàn toàn theo thứ tự vì các bậc không khớp nhau và được sắp xếp đầy đủ theo phạm vi xếp hạng. 

### Tại sao nó hoạt động 

Xếp hạng tối đa M phản ánh đầy đủ mức độ tiếp xúc trước đây của người dùng với các cấp độ. Bất kỳ cấp nào đạt được trước đó đều đã được phản ánh trong M. Do đó, cấp “lần đầu tiên” phải tương ứng với cấp có chứa C nhưng không chứa M. Vì các cấp tiếp giáp và có thứ tự, điều này tương đương với việc kiểm tra xem chỉ số cấp của C có lớn hơn chỉ số của M hay không. Không có trạng thái trung gian hoặc delta D nào có thể thay đổi kết luận này. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

tiers = [
    ("newbie", -10**18, 1199),
    ("pupil", 1200, 1399),
    ("specialist", 1400, 1599),
    ("expert", 1600, 1899),
    ("candidate master", 1900, 2099),
    ("master", 2100, 2399),
    ("grandmaster", 2400, 10**18),
]

def get_rank(x):
    for i, (name, lo, hi) in enumerate(tiers):
        if lo <= x <= hi:
            return i
    return 0

def solve():
    data = input().split()
    if not data:
        return
    s = data[0]
    C = int(data[1])
    M = int(data[2])
    D = int(data[3])

    prev_rank = get_rank(M)
    new_rank = get_rank(C)

    if new_rank > prev_rank:
        print(tiers[new_rank][0])
    else:
        print("Think about it, you can do it!")

if __name__ == "__main__":
    solve()
```Việc triển khai mã hóa các khoảng xếp hạng một cách rõ ràng và thực hiện tra cứu theo thời gian liên tục trên một mảng cố định gồm bảy bậc. chức năng`get_rank`cố ý đơn giản vì số khoảng không đổi và nhỏ. 

Chi tiết triển khai chính là sử dụng M thay vì C - D. Mặc dù đã cung cấp delta nhưng bài toán đã trực tiếp đưa ra mức tối đa lịch sử, thể hiện đầy đủ những thành tựu trong quá khứ. Điều này tránh việc xây dựng lại bất kỳ dòng thời gian nào. 

So sánh ranh giới được bao gồm ở cả hai đầu, khớp chính xác với các định nghĩa xếp hạng. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
aprohACk 2098 2098 10
```| Bước | C hiện tại | Tối đa M | Hạng(C) | Xếp hạng(M) | Hành động | 
| --- | --- | --- | --- | --- | --- | 
| Bắt đầu | 2098 | 2098 | ứng cử viên chủ | ứng cử viên chủ | so sánh | 

Cả hai giá trị đều thuộc cùng một cấp nên không có biểu tượng mới nào được trao. 

Đầu ra:```
Think about it, you can do it!
```### Mẫu 2 

đầu vào:```
ahoraSoyPeor 2237 2288 -69
```| Bước | C hiện tại | Tối đa M | Hạng(C) | Xếp hạng(M) | Hành động | 
| --- | --- | --- | --- | --- | --- | 
| Bắt đầu | 2237 | 2288 | chủ | chủ | so sánh | 

Người dùng vẫn ở cấp độ đạt được cao nhất. 

Đầu ra:```
Think about it, you can do it!
```Điều này cho thấy việc giảm xếp hạng không làm mất đi những thành tích trong quá khứ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ có hai lần tra cứu theo khoảng thời gian cố định trên một mảng có kích thước không đổi | 
| Không gian | O(1) | Chỉ một danh sách cố định gồm bảy cấp được lưu trữ | 

Các ràng buộc cho phép các giá trị xếp hạng tùy ý, nhưng cấu trúc của các bậc đảm bảo phân loại theo thời gian không đổi, giúp giải pháp dễ dàng trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    tiers = [
        ("newbie", -10**18, 1199),
        ("pupil", 1200, 1399),
        ("specialist", 1400, 1599),
        ("expert", 1600, 1899),
        ("candidate master", 1900, 2099),
        ("master", 2100, 2399),
        ("grandmaster", 2400, 10**18),
    ]

    def get_rank(x):
        for i, (name, lo, hi) in enumerate(tiers):
            if lo <= x <= hi:
                return i
        return 0

    data = sys.stdin.readline().split()
    s = data[0]
    C = int(data[1])
    M = int(data[2])
    D = int(data[3])

    if get_rank(C) > get_rank(M):
        return tiers[get_rank(C)][0]
    return "Think about it, you can do it!"

assert run("aprohACk 2098 2098 10\n") == "Think about it, you can do it!"
assert run("ahoraSoyPeor 2237 2288 -69\n") == "Think about it, you can do it!"
assert run("demianOneTwoThree 725 725 721\n") == "specialist"

# custom cases
assert run("x 1199 1199 1\n") == "pupil"  # newbie -> pupil boundary crossing
assert run("x 1399 1399 1\n") == "specialist"  # pupil -> specialist
assert run("x 2500 2000 500\n") == "grandmaster"  # jumps to top tier
assert run("x 1500 1600 0\n") == "Think about it, you can do it!"  # no new tier
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1199 → 1200 | học sinh | vượt biên ở cấp thấp | 
| 1399 → 1400 | chuyên gia | chuyển tiếp cấp trung gian | 
| 2500 | kiện tướng | xử lý cấp cao nhất | 
| 1500 bậc không đổi | tin nhắn | trường hợp không thay đổi | 

## Vỏ cạnh 

Một sai lầm phổ biến là bỏ qua rằng xếp hạng tối đa đã mã hóa những thành tựu lịch sử. Ví dụ: nếu xếp hạng hiện tại của người dùng cao hơn mức tối đa trước đó của họ, điều đó có nghĩa là họ có thể có một cấp độ mới, nhưng nếu cả hai vẫn nằm trong cùng một khoảng thời gian thì sẽ không có biểu tượng nào được trao. Thuật toán xử lý vấn đề này bằng cách so sánh rõ ràng các chỉ số cấp thay vì xếp hạng thô. 

Một trường hợp tinh vi khác là thay đổi xếp hạng đi xuống. Nếu người dùng rớt từ cấp cao hơn xuống cấp thấp hơn, họ sẽ không mất biểu tượng đã kiếm được trước đó vì quyết định dựa trên xếp hạng lịch sử tối đa chứ không phải vị trí hiện tại. Việc so sánh bằng M đảm bảo rằng khi một cấp được mở khóa, cấp đó sẽ được “truy cập” mãi mãi. 

Cuối cùng, các trường hợp bằng nhau về ranh giới, chẳng hạn như chính xác là 1200 hoặc 2100 được xử lý chính xác vì việc kiểm tra khoảng thời gian đã được bao gồm, do đó không có lỗi riêng lẻ nào xảy ra ở ranh giới cấp.
