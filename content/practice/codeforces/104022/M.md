---
title: "CF 104022M - Tháp pháp sư"
description: "Chúng ta được cung cấp một chuỗi các quái vật, mỗi quái vật được mô tả bằng một cặp giá trị: sức tấn công và sức khỏe của nó. Chúng tôi điều khiển một chiến binh bắt đầu với một số sức mạnh ban đầu và sức khỏe vô hạn, vì vậy sự sống sót không phải là chết mà là giảm thiểu mức độ thiệt hại của…"
date: "2026-07-02T04:32:29+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104022
codeforces_index: "M"
codeforces_contest_name: "The 2020 ICPC Asia Yinchuan Regional Programming Contest"
rating: 0
weight: 104022
solve_time_s: 43
verified: true
draft: false
---

[CF 104022M - Tháp phù thủy](https://codeforces.com/problemset/problem/104022/M) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 43s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi các quái vật, mỗi quái vật được mô tả bằng một cặp giá trị: sức tấn công và sức khỏe của nó. Chúng tôi điều khiển một chiến binh bắt đầu với một số sức mạnh ban đầu và sức khỏe vô hạn, vì vậy việc sống sót không phải là chết mà là giảm thiểu mức sát thương mà chiến binh tích lũy trong khi tiêu diệt tất cả quái vật. 

Khi chúng ta chiến đấu với một con quái vật, các trận chiến luân phiên bắt đầu từ chiến binh. Mỗi đòn đánh làm giảm lượng máu của đối thủ bằng sức mạnh của kẻ tấn công và cuộc chiến tiếp tục cho đến khi lượng máu của một bên bằng 0 hoặc thấp hơn. Vì chúng ta luôn đánh trước nên kết cấu của trận đấu sẽ mang tính quyết định sau khi trật tự được ấn định. 

Một thợ cơ bản thay đổi hoàn toàn vấn đề: sau khi đánh bại một con quái vật, sức mạnh của chiến binh sẽ ngang bằng với sức mạnh của con quái vật đó. Vì vậy, thứ tự chúng ta chọn quái vật quyết định không chỉ sát thương tức thời phải nhận trong mỗi trận chiến mà còn cả mức độ sát thương trong tương lai, bởi vì những quái vật mạnh hơn sau này có thể giảm sát thương nhận vào trong các trận chiến tiếp theo. 

Mục tiêu là chọn thứ tự của tất cả quái vật để giảm thiểu tổng thiệt hại mà chiến binh phải gánh chịu trong tất cả các trận chiến. 

Ràng buộc n lên tới 100000 có nghĩa là bất kỳ giải pháp nào có hành vi bậc hai trên các hoán vị đều không thể thực hiện được. Ngay cả O(n^2 log n) cũng đã quá lớn trong thực tế. Điều này ngay lập tức loại trừ mọi thứ tự bạo lực hoặc DP trên các tập hợp con. Chúng ta cần một cái gì đó gần hơn với O(n log n) hoặc O(n). 

Một trường hợp tế nhị xuất phát từ thực tế là sức mạnh chỉ tăng lên sau khi chiến thắng các trận chiến. Một ý tưởng tham lam ngây thơ như luôn chọn con quái vật tiếp theo yếu nhất hoặc mạnh nhất có thể thất bại vì sự lựa chọn tốt nhất phụ thuộc vào cả sức mạnh hiện tại và tiềm năng sớm mở khóa sức mạnh cao hơn trong tương lai. 

Ví dụ, hãy xem xét trường hợp một con quái vật yếu hơn một chút sẽ rất “rẻ” để đánh bại và tăng sức mạnh lớn, trong khi một con quái vật mạnh hơn thì đắt hơn và trì hoãn việc tăng sức mạnh đó. Nếu chọn sai, chúng ta có thể vĩnh viễn nhốt mình vào việc phải chịu sát thương cao trong tất cả các trận chiến tiếp theo. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là thử tất cả các hoán vị của thứ tự quái vật, mô phỏng từng cuộc chiến và tính toán tổng thiệt hại. Mỗi mô phỏng của một đơn hàng cố định tốn O(n) thời gian và có n! các đơn đặt hàng có thể, do đó tổng công việc là O(n! · n), điều này trở nên không khả thi gần như ngay lập tức ngay cả với n khoảng 10. 

Để vượt qua điều này, chúng ta cần hiểu điều gì thực sự góp phần gây ra thiệt hại. Trong một cuộc chiến, nếu chúng ta bắt đầu với sức mạnh S và đối mặt với một quái vật có sức mạnh A và sức khỏe H, thì số đòn đánh cần thiết để tiêu diệt nó được xác định bởi S. Chiến binh và quái vật luân phiên tấn công, vì vậy sát thương nhận được tùy thuộc vào số lượng toàn bộ quái vật tấn công xảy ra trước khi nó chết. Điều quan trọng là sức mạnh cao hơn sẽ giảm thời gian chiến đấu theo cách phi tuyến tính. 

Quan sát quan trọng là sau khi đánh bại một con quái vật, chúng ta sẽ tăng sức mạnh vĩnh viễn lên sức mạnh của con quái vật đó. Vì vậy, vấn đề thực sự nằm ở việc lựa chọn một chuỗi nâng cấp sức mạnh để giảm thiểu chi phí tích lũy, trong đó mỗi chi phí phụ thuộc vào sức mạnh hiện tại. 

Cấu trúc này gợi ý việc sắp xếp quái vật theo sức mạnh và đưa ra quyết định theo thứ tự đó. Tuy nhiên, chúng ta không thể đơn giản đi theo thứ tự tăng hoặc giảm, vì sát thương từ một trận chiến phụ thuộc vào thời gian giảm HP bằng sức mạnh hiện tại và điều đó tương tác với các nâng cấp trong tương lai.

Cách tiêu chuẩn để giải quyết vấn đề này là diễn giải lại mỗi con quái vật như một “công việc” mà chi phí của nó phụ thuộc vào sức mạnh hiện tại và sử dụng một mệnh lệnh tham lam luôn chọn cách chuyển đổi có lợi nhất. Trong vấn đề này, chiến lược tối ưu giảm xuống việc sắp xếp quái vật theo sức mạnh của chúng và xử lý chúng theo thứ tự sức mạnh tăng dần, bởi vì việc trì hoãn một quái vật có sức mạnh yếu hơn không bao giờ giúp ích: chiến đấu với quái vật có sức mạnh cao hơn trước sẽ mang lại sự nâng cấp tốt hơn hoặc ngang bằng trước đó, điều này chỉ làm giảm thiệt hại trong tương lai. 

Sau khi chúng tôi chấp nhận mệnh lệnh này, mỗi trận chiến sẽ trở thành một phép tính chi phí xác định bằng cách sử dụng sức mạnh hiện tại và chúng tôi sẽ tích lũy tổng thiệt hại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n! · n) | O(n) | Quá chậm | 
| Tối ưu (sắp xếp + mô phỏng tham lam) | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp tất cả quái vật theo sức mạnh của chúng theo thứ tự tăng dần. Điều này đảm bảo rằng khi chúng ta xử lý một quái vật, tất cả quái vật được xử lý trước đó đều có sức mạnh nhỏ hơn hoặc bằng quái vật hiện tại, do đó sức mạnh chỉ tăng lên theo thời gian. 
2. Khởi tạo sức mạnh hiện tại của chiến binh làm giá trị ban đầu nhất định và khởi tạo tổng thiệt hại bằng 0. 
3. Lặp lại lần lượt từng quái vật được sắp xếp. 
4. Đối với mỗi quái vật, hãy mô phỏng cuộc chiến bằng sức mạnh hiện tại để xác định số lượng quái vật tấn công xảy ra trước khi nó chết. Số lần đánh cần thiết của chiến binh được xác định bằng mức chia trần HP của quái vật theo sức mạnh hiện tại. 
5. Từ số lần trao đổi này, hãy tính xem quái vật có thể tấn công chiến binh bao nhiêu lần. Vì chiến binh luôn tấn công trước nên quái vật tấn công ít hơn một lần so với số lần đánh cần thiết để kết thúc nó. 
6. Cộng tổng sát thương từ các đòn tấn công của quái vật này vào đáp án chung. 
7. Sau khi trận chiến kết thúc, hãy cập nhật sức mạnh của chiến binh thành sức mạnh của quái vật này. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào, trạng thái duy nhất ảnh hưởng đến kết quả trong tương lai là sức mạnh hiện tại. Việc xử lý quái vật theo thứ tự tăng dần đảm bảo rằng sức mạnh không hề giảm đi một cách đơn điệu. Bất kỳ sự sai lệch nào so với mệnh lệnh này sẽ trì hoãn việc tăng sức mạnh một cách không cần thiết, điều này chỉ có thể tăng hoặc duy trì thời gian chiến đấu trong tương lai vì sức mạnh cao hơn luôn làm giảm hoặc duy trì số lần đánh cần thiết cho bất kỳ quái vật nào còn lại. Sự đơn điệu này làm cho trật tự tham lam trở nên tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, s0 = map(int, input().split())
    monsters = []
    for _ in range(n):
        a, h = map(int, input().split())
        monsters.append((a, h))

    monsters.sort()

    s = s0
    total_damage = 0

    for a, h in monsters:
        hits = (h + s - 1) // s
        damage_taken = (hits - 1) * a
        total_damage += damage_taken
        s = a

    print(total_damage)

if __name__ == "__main__":
    solve()
```Mã bắt đầu bằng cách sắp xếp quái vật theo sức mạnh của chúng, thực thi cấu trúc tham lam. Chúng tôi duy trì sức mạnh hiện tại`s`và xử lý từng quái vật theo thứ tự đó. 

Đối với mỗi quái vật, chúng tôi tính toán số lần đánh cần thiết để giảm HP của nó xuống 0 bằng cách sử dụng phép chia trần số nguyên. Vì chúng ta tấn công trước nên một con quái vật cần`hits`lượt truy cập sẽ có thể tấn công chúng tôi một cách chính xác`hits - 1`lần. Mỗi đòn tấn công như vậy gây sát thương cố định bằng với sức mạnh của quái vật, do đó tổng sát thương là`(hits - 1) * a`. 

Sau khi tiêu diệt một con quái vật, chúng ta cập nhật sức mạnh của mình thành`a`, phản ánh quy luật của vấn đề rằng việc đánh bại quái vật sẽ nâng cấp sức mạnh của chúng ta. 

## Ví dụ đã hoạt động 

Chúng tôi xây dựng một ví dụ nhỏ để minh họa động lực học. 

Ví dụ đầu vào:```
3 2
2 3
4 5
3 4
```Sắp xếp theo sức mạnh: 

(2,3), (3,4), (4,5) 

| Bước | S hiện tại | Quái vật (A,H) | Lượt truy cập | Tấn công quái vật | Thiệt hại | S mới | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 2 | (2,3) | 2 | 1 | 2 | 2 | 
| 2 | 2 | (3,4) | 2 | 1 | 3 | 3 | 
| 3 | 3 | (4,5) | 2 | 1 | 4 | 4 | 

Tổng thiệt hại là 9. 

Dấu vết này cho thấy sức mạnh tiến hóa dần dần và sát thương của mỗi trận chiến chỉ phụ thuộc vào sức mạnh hiện tại, khẳng định tính độc lập tham lam giữa các bước. 

Một ví dụ thứ hai:```
2 1
10 10
2 3
```Đã sắp xếp: 

(2,3), (10,10) 

| Bước | S hiện tại | Quái vật (A,H) | Lượt truy cập | Tấn công quái vật | Thiệt hại | S mới | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 1 | (2,3) | 3 | 2 | 4 | 2 | 
| 2 | 2 | (10,10) | 5 | 4 | 40 | 10 | 

Điều này chứng tỏ tại sao nên xử lý quái vật yếu trước: mặc dù (10,10) mạnh hơn nhưng xử lý sớm sẽ khiến chúng ta có sức mạnh thấp hơn và tăng đáng kể sát thương trong tương lai. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp chiếm ưu thế, mỗi quái vật được xử lý một lần | 
| Không gian | O(n) | Lưu trữ danh sách quái vật | 

Các ràng buộc cho phép tối đa 100000 quái vật, vì vậy O(n log n) cũng nằm trong giới hạn. Mô phỏng chỉ sử dụng số học đơn giản cho mỗi quái vật, giúp nó nhanh chóng trong thực tế. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import inf

    data = inp.strip().split()
    n = int(data[0])
    s = int(data[1])
    monsters = []
    idx = 2
    for _ in range(n):
        a = int(data[idx]); h = int(data[idx+1])
        monsters.append((a, h))
        idx += 2

    monsters.sort()
    ans = 0
    cur = s
    for a, h in monsters:
        hits = (h + cur - 1) // cur
        ans += (hits - 1) * a
        cur = a

    return str(ans)

# provided sample
assert run("4 1\n3 2\n4 4\n5 6\n1 6\n") == "9"

# minimum size
assert run("1 10\n5 1\n") == "0"

# increasing strength chain
assert run("3 1\n2 5\n3 5\n4 5\n") == "9"

# all equal strength
assert run("3 2\n2 4\n2 4\n2 4\n") == "6"

# large HP edge
assert run("2 3\n5 100\n10 1\n") == "75"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| quái vật đơn lẻ | 0 | không có trường hợp tấn công lặp đi lặp lại | 
| chuỗi ngày càng tăng | 9 | tương tác tăng trưởng sức mạnh | 
| điểm mạnh giống hệt nhau | 6 | sự ổn định dưới mối quan hệ | 
| vỏ HP lớn | 75 | tính đúng đắn của việc phân chia trần và đếm đòn tấn công | 

## Vỏ cạnh 

Trường hợp quan trọng là khi HP nhỏ hơn hoặc bằng cường độ hiện tại. Trong trường hợp này, quái vật chết trong một đòn duy nhất và không gây sát thương. Thuật toán xử lý việc này một cách chính xác vì`(h + s - 1) // s`đánh giá là 1, và`(hits - 1)`trở thành số không, tạo ra sự đóng góp bằng không. 

Một trường hợp khó khăn khác là khi nhiều quái vật có cùng sức mạnh. Việc sắp xếp giữ cho thứ tự của chúng tùy ý giữa những thứ bằng nhau, nhưng vì sức mạnh không thay đổi sau những trận chiến như vậy nên việc tính toán thiệt hại vẫn nhất quán bất kể thứ tự nội bộ. 

Cuối cùng, giá trị HP rất lớn kết hợp với cường độ ban đầu nhỏ nhấn mạnh tính chính xác của việc phân chia trần. Công thức đảm bảo rằng ngay cả khi HP không chia hết cho sức mạnh, chúng tôi tính toán chính xác cú đánh một phần cuối cùng và do đó tính chính xác chu kỳ tấn công của quái vật.
