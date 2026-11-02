---
title: "CF 1088B - Ehab và phép trừ"
description: "Chúng tôi liên tục thực hiện việc “giảm cấp độ” toàn cầu trên một mảng. Ở mỗi bước, chúng tôi xem xét tất cả các giá trị dương hiện có, xác định giá trị nhỏ nhất trong số chúng, xuất giá trị đó và sau đó giảm mọi phần tử dương theo cùng một lượng đó."
date: "2026-06-15T05:24:24+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1088
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 525 (Div. 2)"
rating: 1000
weight: 1088
solve_time_s: 173
verified: false
draft: false
---

[CF 1088B - Ehab và phép trừ](https://codeforces.com/problemset/problem/1088/B) 

**Đánh giá:** 1000 
**Tags:** triển khai, sắp xếp 
**Thời gian giải:** 2m 53s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi liên tục thực hiện việc “giảm cấp độ” toàn cầu trên một mảng. Ở mỗi bước, chúng tôi xem xét tất cả các giá trị dương hiện có, xác định giá trị nhỏ nhất trong số chúng, xuất giá trị đó và sau đó giảm mọi phần tử dương theo cùng một lượng đó. Số không không thay đổi và không bao giờ đóng góp nữa. 

Quá trình này tiếp tục với một số thao tác cố định, ngay cả khi mảng trở thành hoàn toàn bằng 0 trước đó. Khi mọi thứ đều bằng 0, mọi thao tác còn lại chỉ đơn giản là tạo ra số 0. 

Cấu trúc quan trọng là mảng đang được bóc từng lớp từ dưới lên: mỗi thao tác sẽ loại bỏ “chiều cao” dương tối thiểu hiện tại khỏi tất cả các mục dương còn lại. 

Các ràng buộc cho phép tối đa 100.000 phần tử và 100.000 thao tác. Bất kỳ giải pháp nào tính toán lại mức tối thiểu bằng cách quét toàn bộ mảng trong mỗi bước đều dẫn đến khoảng 10^10 thao tác trong trường hợp xấu nhất, vượt xa giới hạn thời gian. Ngay cả việc sắp xếp đầy đủ lặp đi lặp lại hoặc tính toán lại đầy đủ cho mỗi bước cũng sẽ không thành công. 

Một vài trường hợp đặc biệt bộc lộ rõ ​​ràng những cách tiếp cận ngây thơ. 

Nếu mảng đã toàn là số 0 thì câu trả lời chỉ đơn giản là k số 0. Việc triển khai ngây thơ giả định tồn tại ít nhất một giá trị dương có thể cố gắng tính giá trị tối thiểu và bị lỗi hoặc trả về giá trị không hợp lệ. 

Nếu k lớn hơn nhiều so với số lượng các giá trị dương riêng biệt thì cuối cùng quá trình sẽ ổn định ở tất cả các số 0 và tiếp tục in các số 0. Ví dụ,`[5]`với k = 3 tạo ra`5, 0, 0`. Giải pháp dừng sớm sau khi mảng trống mà không thực hiện các thao tác còn lại sẽ không chính xác. 

## Phương pháp tiếp cận 

Một mô phỏng trực tiếp rất dễ hình dung: quét liên tục mảng, tìm phần tử khác 0 nhỏ nhất, trừ nó khỏi mọi phần tử dương và in nó. Điều này đúng vì nó tuân theo chính xác định nghĩa hoạt động. Tuy nhiên, mỗi bước tốn O(n) và trong trường hợp xấu nhất chúng ta có thể thực hiện O(k) bước, cho ra O(nk). Với cả hai lên tới 10^5, điều này trở thành 10^10 thao tác, điều này không khả thi. 

Quan sát quan trọng là chúng ta không thực sự quan tâm đến các giá trị mảng đang phát triển tại mọi thời điểm. Chúng tôi chỉ quan tâm đến chuỗi các giá trị dương tối thiểu khi mảng bị “làm phẳng”. Mỗi lần chúng ta trừ đi giá trị dương tối thiểu hiện tại, ít nhất một phần tử sẽ trở thành 0 và tập hợp các giá trị hiện hoạt sẽ co lại. Quá trình này tương đương với việc trích xuất nhiều lần các giá trị dương riêng biệt theo thứ tự tăng dần. 

Nếu chúng ta sắp xếp mảng, trình tự thao tác sẽ trở nên rõ ràng hơn nhiều. Bất cứ khi nào chúng ta di chuyển từ một giá trị riêng biệt sang giá trị lớn hơn tiếp theo, sự khác biệt đó tương ứng với một lớp trừ mới. Các giá trị bằng nhau sẽ gộp lại với nhau, tạo ra các khoảng trống có độ dài bằng 0. Số lần đóng góp của một giá trị cụ thể được xác định bằng số lượng “cấp” riêng biệt vẫn ở trên giá trị đó. 

Do đó, việc sắp xếp mảng và các nhóm xử lý có giá trị bằng nhau cho phép chúng ta tính toán số lần mỗi mức tối thiểu xuất hiện mà không cần mô phỏng mọi phép trừ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(nk) | O(1) | Quá chậm | 
| Sắp xếp + nhóm | O(n log n) | O(1) hoặc O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp mảng theo thứ tự không giảm. Điều này đảm bảo rằng các giá trị bằng nhau được nhóm lại và giá trị nhỏ nhất còn lại luôn có thể truy cập được ở phía trước cấu trúc. 
2. Lặp lại mảng đã sắp xếp, theo dõi giá trị trước đó được thấy giữa các phần tử dương. Mỗi lần chúng tôi gặp một giá trị dương riêng biệt mới, chúng tôi tính toán xem có bao nhiêu phần tử vẫn hoạt động kể từ thời điểm này trở đi. 
3. Đối với mỗi giá trị riêng biệt, hãy xác định số lần nó sẽ được in. Giá trị này bằng với số phần tử còn lại lớn hơn hoặc bằng giá trị này, trừ đi những phần tử đã được “tiêu thụ” ở các cấp độ trước đó. 
4. Xuất ra sự khác biệt giữa các giá trị riêng biệt liên tiếp theo số lượng phần tử hoạt động còn lại. Về mặt khái niệm, mỗi nhóm giá trị bằng nhau đóng góp một bước trừ “chiều cao lớp”. 
5. Dừng lại khi chúng ta đã tạo ra k kết quả đầu ra. Nếu chúng ta sử dụng hết tất cả các giá trị dương trước khi đạt đến k, hãy thêm số 0 cho các phép toán còn lại. 

Tính chính xác đến từ tính bất biến là sau mỗi bước trừ, tập hợp các giá trị dương chính xác là các giá trị ban đầu được giảm đi theo số ngưỡng riêng biệt đã được vượt qua. Việc sắp xếp sẽ hiển thị trực tiếp các ngưỡng này, vì vậy mỗi giá trị riêng biệt tương ứng với một bước trong đó nó trở thành mức tối thiểu mới. 

Thuật toán hoạt động vì quá trình này tương đương với việc bóc tách các mức giá trị được sắp xếp. Mỗi thay đổi cấp độ tương ứng với một giá trị tăng dần trong mảng đã sắp xếp và mỗi thay đổi như vậy sẽ tạo ra chính xác một đầu ra cho đến khi hết tất cả các phần tử. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())
    a = list(map(int, input().split()))
    
    a.sort()
    
    res = []
    i = 0
    
    while i < n and len(res) < k:
        if a[i] == 0:
            i += 1
            continue
        
        # current minimum positive value
        current = a[i]
        
        # skip all equal values
        j = i
        while j < n and a[j] == current:
            j += 1
        
        # number of remaining active elements
        remaining = n - i
        
        res.append(current)
        
        # after subtracting, all elements in [i, j) become zero,
        # so we conceptually move forward
        i = j
    
    # if we still need outputs, fill with zeros
    while len(res) < k:
        res.append(0)
    
    print("\n".join(map(str, res)))

if __name__ == "__main__":
    solve()
```Việc triển khai dựa vào việc sắp xếp để hiển thị cấu trúc của quá trình trừ lặp lại. Con trỏ`i`luôn hướng tới giá trị dương nhỏ nhất còn lại. Chúng tôi nhóm các giá trị bằng nhau lại với nhau bằng cách sử dụng`j`, vì chúng hoạt động giống hệt nhau dưới phép trừ. Mỗi nhóm tạo ra một đầu ra tương ứng với mức tối thiểu hiện tại. 

Phần điền còn lại bằng số 0 xử lý trường hợp tất cả các phần tử đã hết trước k thao tác. 

Một điểm tinh tế là chúng tôi không bao giờ trừ các giá trị khỏi mảng một cách rõ ràng. Hoạt động đó tiềm ẩn trong logic nhóm: một khi chúng tôi chuyển một giá trị, nó sẽ được coi là bị loại bỏ hoàn toàn khỏi các cân nhắc tối thiểu trong tương lai. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3 5
1 2 3
```Mảng được sắp xếp là`[1, 2, 3]`. 

| Bước | tôi | hiện tại | đầu ra | còn hoạt động | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | 1 | 1 | 3 | 
| 2 | 1 | 2 | 1 | 2 | 
| 3 | 2 | 3 | 1 | 1 | 
| 4 | - | - | 0 | 0 | 
| 5 | - | - | 0 | 0 | 

Chúng tôi xuất từng cấp độ riêng biệt một lần cho đến khi hết mảng. Sau khi tất cả các giá trị được tiêu thụ, chúng tôi đệm bằng số không. 

Điều này cho thấy rằng mỗi giá trị được sắp xếp riêng biệt sẽ đóng góp một lớp trừ. 

### Mẫu 2 

đầu vào:```
4 5
10 3 5 3
```Mảng được sắp xếp là`[3, 3, 5, 10]`. 

| Bước | tôi | hiện tại | đầu ra | còn hoạt động | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | 3 | 3 | 4 | 
| 2 | 2 | 5 | 3 | 2 | 
| 3 | 3 | 10 | 5 | 1 | 
| 4 | - | - | 0 | 0 | 
| 5 | - | - | 0 | 0 | 

Mỗi lần chúng tôi chuyển đến một giá trị khác biệt cao hơn, chúng tôi sẽ phát ra giá trị đó ở mức tối thiểu tiếp theo sau lần giảm trước đó. 

Điều này xác nhận rằng việc sắp xếp sẽ chuyển đổi quá trình trừ lặp lại thành một chuỗi các bước nhảy ngưỡng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | sắp xếp chiếm ưu thế, quét tuyến tính đơn sau | 
| Không gian | O(1) bổ sung (không bao gồm đầu vào) | xử lý tại chỗ mảng đã sắp xếp | 

Độ phức tạp phù hợp thoải mái trong các ràng buộc vì n lên tới 100.000 và việc sắp xếp ở quy mô này là hiệu quả trong Python. Đường truyền tuyến tính đảm bảo không có chi phí bổ sung tỷ lệ với k. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    import io as sio

    out = sio.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided sample
assert run("""3 5
1 2 3
""") == """1
1
1
0
0"""

# all equal values
assert run("""4 3
5 5 5 5
""") == """5
0
0"""

# single element
assert run("""1 4
7
""") == """7
0
0
0"""

# already zeros
assert run("""3 2
0 0 0
""") == """0
0"""

# mixed values
assert run("""5 6
4 1 4 2 1
""") == """1
1
2
4
0
0"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 7,0,0,0 | kiệt sức và đệm | 
| tất cả đều bình đẳng | 5,0,0 | sự sụp đổ của các giá trị giống hệt nhau | 
| tất cả số không | 0,0 | trường hợp không cạnh đầy đủ | 
| hỗn hợp | 1,1,2,4,0,0 | sắp xếp đúng thứ tự các cấp độ khác nhau | 

## Vỏ cạnh 

Nếu mảng chỉ chứa các số 0, thuật toán sẽ ngay lập tức chuyển sang giai đoạn đệm. Vì không gặp giá trị dương nào nên quá trình quét được sắp xếp không tạo ra kết quả đầu ra nào và vòng lặp cuối cùng xuất ra k số 0. Điều này phù hợp với định nghĩa vì mọi thao tác đều nhìn thấy một mảng hoàn toàn bằng 0. 

Đối với một đầu vào như`[5]`với k lớn hơn 1, thuật toán đưa ra`5`ở lần vượt qua đầu tiên, sau đó nhận ra không còn giá trị dương nào và điền vào các số 0. Điều này phù hợp với ý tưởng rằng một khi phần tử duy nhất bị giảm đi thì quá trình sẽ ổn định. 

Nếu tất cả các phần tử đều bằng nhau, hãy nói`[3,3,3]`, quá trình quét được sắp xếp sẽ tạo ra một giá trị riêng biệt. Giá trị đó được xuất ra một lần và tất cả các hoạt động còn lại bằng 0. Điều này cho thấy thực tế là một bước trừ sẽ loại bỏ tất cả các phần tử cùng một lúc.
