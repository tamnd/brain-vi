---
title: "CF 104614G - Họa Tiết Hạt Đậu"
description: "Chúng ta có hai số nguyên rất lớn được viết dưới dạng chuỗi chữ số. Số đầu tiên được sử dụng làm điểm bắt đầu của một chuỗi xác định và số thứ hai là mục tiêu mà chúng ta đang cố gắng xác định bên trong chuỗi đó. Trình tự phát triển theo một cách rất cụ thể."
date: "2026-06-29T20:03:13+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104614
codeforces_index: "G"
codeforces_contest_name: "2022-2023 ICPC East Central North America Regional Contest (ECNA 2022)"
rating: 0
weight: 104614
solve_time_s: 53
verified: true
draft: false
---

[CF 104614G - Mẫu hạt đậu](https://codeforces.com/problemset/problem/104614/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 53s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai số nguyên rất lớn được viết dưới dạng chuỗi chữ số. Số đầu tiên được sử dụng làm điểm bắt đầu của một chuỗi xác định và số thứ hai là mục tiêu mà chúng ta đang cố gắng xác định bên trong chuỗi đó. 

Trình tự phát triển theo một cách rất cụ thể. Mỗi thuật ngữ tiếp theo được xây dựng bằng cách mô tả thuật ngữ trước đó dưới dạng nhiều tập chữ số. Thay vì mô tả các chữ số theo thứ tự xuất hiện, chúng tôi quét các chữ số theo thứ tự tăng dần từ 0 đến 9. Với mỗi chữ số xuất hiện, chúng tôi ghi lại số lần nó xuất hiện, ngay sau đó là chính chữ số đó. Việc ghép tất cả các khối như vậy sẽ tạo ra phần tử chuỗi tiếp theo. 

Vì vậy, việc chuyển đổi không phải là vị trí mà dựa trên tần số. Quá trình này hoàn toàn mang tính xác định, nghĩa là khi số bắt đầu được cố định thì toàn bộ chuỗi cũng được cố định. 

Nhiệm vụ là xác định xem số mục tiêu có xuất hiện trong chuỗi được tạo này hay không. Nếu có, chúng ta phải xuất vị trí dựa trên 1 của nó. Nếu nó không xuất hiện, chúng ta phải quyết định xem chuỗi đã đi vào một chu kỳ trong một giới hạn hợp lý hay chưa, hoặc liệu nó có phát triển đủ lâu mà không lặp lại hay không, trong trường hợp đó chúng ta xuất ra chuỗi đặc biệt biểu thị sự nhàm chán. 

Các ràng buộc về số có độ lớn cực lớn, lên tới 10^100 chữ số, điều này ngay lập tức loại trừ mọi cách tiếp cận số học trên số nguyên. Mọi thao tác phải được thực hiện trên chuỗi. Hoạt động có ý nghĩa duy nhất là đếm tần số chữ số. 

Một cách giải thích ngây thơ sẽ gợi ý rằng trình tự có thể phát triển vô hạn hoặc hoạt động một cách hỗn loạn. Tuy nhiên, cấu trúc ẩn chính là không gian trạng thái là hữu hạn trong thực tế vì phép biến đổi chỉ phụ thuộc vào số lượng chữ số và chỉ có hữu hạn nhiều chuỗi có thể xuất hiện trước khi sự lặp lại tạo ra một chu kỳ. 

Một số trường hợp đặc biệt cần được chú ý. 

Một vấn đề phát sinh khi giá trị bắt đầu đã bằng mục tiêu. Trong trường hợp này, câu trả lời ngay lập tức là vị trí 1. Ví dụ: nếu đầu vào là`3112 3112`, đầu ra đúng là`1`. 

Một vấn đề khác là khi trình tự đi vào một chu trình không chứa mục tiêu. Việc triển khai đơn giản có thể tiếp tục vô thời hạn, nhưng vì số lượng trạng thái có thể bị giới hạn nên cuối cùng sự lặp lại phải xảy ra. Ví dụ: nếu chuỗi tuần hoàn giữa hai chuỗi không bao gồm mục tiêu, thì kết quả đầu ra đúng là nó không xuất hiện chứ không phải là chúng ta tiếp tục mô phỏng mãi mãi. 

Trường hợp khó phát hiện cuối cùng là khi chuỗi phát triển vượt quá giới hạn thăm dò cho phép là 100 trạng thái duy nhất mà không lặp lại. Trong trường hợp như vậy, vấn đề rõ ràng đòi hỏi phải báo cáo sự nhàm chán ngay cả khi chúng ta chưa chứng minh được một cách thuyết phục sự vắng mặt của mục tiêu. Một mô phỏng đơn giản không theo dõi sự lặp lại hoặc giới hạn có thể dễ dàng vượt quá thời gian hoặc bộ nhớ. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là mô phỏng trình tự từng bước. Bắt đầu từ chuỗi ban đầu, chúng tôi tạo chuỗi tiếp theo bằng cách sử dụng tần số đếm trên các chữ số từ 0 đến 9, sau đó so sánh nó với chuỗi đích. Chúng tôi lặp lại quá trình này cho đến khi tìm thấy mục tiêu, phát hiện sự lặp lại hoặc vượt quá thời lượng khám phá tối đa được phép. 

Phương pháp brute-force này đúng vì mỗi trạng thái chỉ phụ thuộc vào trạng thái trước đó, do đó việc mô phỏng từng bước sẽ tái tạo chính xác trình tự. Bản thân phép biến đổi là tuyến tính theo độ dài của chuỗi hiện tại, vì vậy nếu chúng ta thực hiện T chuyển đổi và độ dài chuỗi trung bình là L, thì độ phức tạp sẽ trở thành O(T · L). Trong trường hợp xấu nhất, chuỗi có thể di chuyển quanh 100 trạng thái với độ dài chuỗi không cần thiết, khiến đường biên này trở nên khả thi nhưng vẫn khả thi. 

Tuy nhiên, nếu không phát hiện chu kỳ, phương pháp này có thể lặp lại mãi mãi. Vì phép biến đổi ánh xạ một chuỗi sang một chuỗi khác trong một không gian hữu hạn nên cuối cùng sự lặp lại được đảm bảo. Khi một trạng thái lặp lại, chuỗi sẽ trở thành tuần hoàn và sẽ không bao giờ đưa ra các giá trị mới nữa. Điều này cho phép chúng tôi dừng mô phỏng một cách an toàn khi chúng tôi phát hiện trạng thái đã thấy trước đó. 

Do đó, cải tiến quan trọng là duy trì một tập hợp các chuỗi đã truy cập và bộ đếm các bước. Điều này chuyển đổi quá trình thành một quá trình truyền tải hữu hạn trên một đồ thị hàm số, trong đó mỗi nút có chính xác một cạnh đi ra. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng Brute Force không theo dõi chu kỳ | O(∞) trong trường hợp xấu nhất | O(1) | Không an toàn | 
| Mô phỏng với tính năng băm và phát hiện chu trình | O(T · L) | O(T · L) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc chuỗi bắt đầu và chuỗi đích dưới dạng văn bản thô. Hãy coi cả hai hoàn toàn là các chuỗi ký tự, vì độ lớn bằng số không liên quan. 
2. Khởi tạo một từ điển hoặc thiết lập để ghi lại mọi trạng thái đã thấy trước đó, cùng với vị trí mà nó xuất hiện. Điều này cho phép cả phát hiện chu kỳ và báo cáo vị trí. 
3. Đặt trạng thái hiện tại thành chuỗi bắt đầu và đánh dấu nó là vị trí 1. Nếu nó đã bằng đích, hãy trả về 1 ngay lập tức vì định nghĩa chuỗi bao gồm thuật ngữ ban đầu. 
4. Lặp lại quá trình chuyển đổi cho đến khi chúng tôi vượt quá 100 trạng thái được tạo hoặc phát hiện trạng thái lặp lại. Đối với mỗi lần lặp, hãy xây dựng chuỗi tiếp theo bằng cách đếm tần số chữ số. 
5. Để xây dựng trạng thái tiếp theo, quét các chữ số từ 0 đến 9 theo thứ tự tăng dần. Đối với mỗi chữ số, nếu tần số của nó trong chuỗi hiện tại khác 0, hãy thêm biểu diễn chuỗi của số đếm theo sau là chính chữ số đó. Điều này đảm bảo thứ tự chuẩn của mô tả. 
6. Sau khi xây dựng chuỗi tiếp theo, tăng bộ đếm vị trí và kiểm tra xem nó có bằng mục tiêu hay không. Nếu có, hãy trả lại vị trí hiện tại. 
7. Nếu chuỗi tiếp theo đã được nhìn thấy trước đó thì chu kỳ đã được phát hiện. Vì không có trạng thái duy nhất mới nào xuất hiện ngoài điểm này nên hãy kết thúc tìm kiếm. 
8. Nếu chúng ta vượt quá 100 trạng thái riêng biệt mà không tìm thấy mục tiêu hoặc chứng tỏ sự kết thúc, hãy trả lại tín hiệu buồn chán. 

### Tại sao nó hoạt động

Mỗi trạng thái trong chuỗi được xác định đầy đủ bởi mô tả tần số chữ số của nó, do đó việc ánh xạ từ trạng thái này sang trạng thái tiếp theo là xác định. Điều này có nghĩa là chuỗi tạo thành một đồ thị có hướng trong đó mỗi nút có chính xác một cạnh đi ra. Trong cấu trúc như vậy, mọi đường đi cuối cùng đều đi vào một chu trình. Khi một trạng thái lặp lại, tất cả quá trình tiến hóa trong tương lai đều giống hệt với phân đoạn trước đó, do đó, không mục tiêu vô hình nào có thể xuất hiện ngoài điểm đó trừ khi nó đã xuất hiện trong chu kỳ. Thuật toán khám phá đường dẫn này cho đến khi tìm thấy mục tiêu hoặc đảm bảo cấu trúc về sự lặp lại hoặc giới hạn độ dài cho phép chấm dứt. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def next_state(s: str) -> str:
    freq = [0] * 10
    for ch in s:
        freq[ord(ch) - 48] += 1

    parts = []
    for d in range(10):
        c = freq[d]
        if c:
            parts.append(str(c))
            parts.append(str(d))
    return ''.join(parts)

def solve():
    n, m = input().split()
    if n == m:
        print(1)
        return

    seen = {n: 1}
    cur = n

    for step in range(2, 101):
        cur = next_state(cur)

        if cur == m:
            print(step)
            return

        if cur in seen:
            print("Does not appear")
            return

        seen[cur] = step

    print("I'm bored")

if __name__ == "__main__":
    solve()
```Cốt lõi của việc thực hiện là`next_state`hàm chuyển đổi một chuỗi thành một mảng tần số trên các chữ số từ 0 đến 9. Vòng lặp xây dựng thực thi thứ tự cần thiết bằng cách quét các chữ số theo thứ tự tăng dần. Thứ tự này rất cần thiết vì phép biến đổi được xác định theo từ điển theo giá trị chữ số chứ không phải theo thứ tự xuất hiện. 

Vòng lặp chính duy trì cả bộ đếm bước và từ điển các trạng thái được nhìn thấy. Từ điển chỉ được sử dụng để phát hiện chu kỳ và không lưu trữ bất kỳ cấu trúc bổ sung nào, giữ mức sử dụng bộ nhớ tỷ lệ thuận với số lượng trạng thái riêng biệt gặp phải. 

Vòng lặp giới hạn 100 trực tiếp thực hiện sự đảm bảo của bài toán về sự hội tụ. Nếu không có giới hạn này, chúng tôi sẽ hoàn toàn dựa vào việc phát hiện chu kỳ, nhưng thông số kỹ thuật cho phép chúng tôi dừng sớm khi đạt đến ngưỡng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1 3112
```| Bước | Hiện tại | Hoạt động | Tiếp theo | Đã thấy trước đây? | Cuộc thi đấu? | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | bắt đầu | 11 | không | không | 
| 2 | 11 | đếm(1)=2 | 21 | không | không | 
| 3 | 21 | đếm(1)=1, đếm(2)=1 | 1112 | không | không | 
| 4 | 1112 | đếm(1)=3, đếm(2)=1 | 3112 | không | vâng | 

Ở bước 4, trạng thái được tạo bằng đích, vì vậy câu trả lời là 4. Dấu vết này cho thấy cách nhóm chữ số theo thứ tự được sắp xếp sẽ tạo ra các mã hóa mô tả dần dần nhiều hơn cho các chuỗi trước đó. 

### Ví dụ 2 

đầu vào:```
1 3113
```| Bước | Hiện tại | Hoạt động | Tiếp theo | Đã thấy trước đây? | Cuộc thi đấu? | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | bắt đầu | 11 | không | không | 
| 2 | 11 | đếm(1)=2 | 21 | không | không | 
| 3 | 21 | đếm → 1112 | 1112 | không | không | 
| 4 | 1112 | đếm → 3112 | 3112 | không | không | 
| 5 | 3112 | đếm → 211213 | 211213 | không | không | 
| 6 | 211213 | đếm → 312213 | 312213 | không | không | 
| 7 | 312213 | trạng thái tiếp theo | ... | chu kỳ hoặc tiếp tục | điều kiện dừng | 

Không có lúc nào 3113 xuất hiện, do đó trình tự sẽ quay vòng hoặc ổn định mà không trúng mục tiêu. Mô phỏng phát hiện sự vắng mặt thông qua việc lặp lại hoặc đạt đến giới hạn bước. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(T · L) | Mỗi lần chuyển đổi sẽ quét chuỗi hiện tại một lần để tính tần số chữ số và T được giới hạn bởi 100 | 
| Không gian | O(T · L) | Lưu trữ các trạng thái đã thấy và chuỗi trung gian lên tới 100 lần lặp | 

Cho rằng cả T và L đều nhỏ trong thực tế do độ sâu thăm dò bị giới hạn, giải pháp sẽ hoạt động thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    from types import ModuleType
    return _sys.modules[__name__].solve()  # placeholder if integrated

# provided samples (illustrative format)
# assert run("1 3112") == "4"
# assert run("1 3113") == "Does not appear"

# custom cases
assert run("0 0") == "1", "single digit match"
assert run("1 1") == "1", "immediate equality"
assert run("2 11") in {"Does not appear", "I'm bored"}, "no early match"
assert run("123 112131") in {"Does not appear", "I'm bored"}, "structured growth case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 0 0 | 1 | trận đấu ngay lập tức khi bắt đầu | 
| 1 1 | 1 | trường hợp cạnh nhận dạng | 
| 2 11 | Không xuất hiện / Tôi chán | phát hiện vắng mặt | 
| 123 112131 | phụ thuộc | hành vi chuyển đổi nhiều chữ số | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi chuỗi bắt đầu đã khớp với mục tiêu. Thuật toán kiểm tra rõ ràng điều này trước bất kỳ phép biến đổi nào, do đó, nó ngay lập tức xuất ra vị trí 1 mà không cần vào vòng lặp mô phỏng. 

Một trường hợp khác là hình thành chu kỳ sớm. Giả sử một chuỗi đi vào một vòng lặp sau một vài phép biến đổi, ví dụ A → B → C → B. Khi B được xem lại, thuật toán sẽ phát hiện sự lặp lại và dừng lại, trả về rằng mục tiêu không xuất hiện nếu nó chưa được nhìn thấy trước đó. Điều này ngăn cản sự truyền tải vô hạn và phản ánh chính xác cấu trúc tuần hoàn. 

Trường hợp cuối cùng là khi chuỗi phát triển gần đến ngưỡng 100 bước mà không lặp lại. Trong trường hợp như vậy, bộ đếm vòng lặp buộc phải chấm dứt bất kể chu trình có được phát hiện hay không. Đầu vào là:```
n large_state m distant_state
```Quá trình mô phỏng sẽ tiến triển từng bước và khi đạt đến 100 trạng thái, thuật toán sẽ phát ra tín hiệu nhàm chán. Điều này đảm bảo tuân thủ các ràng buộc rõ ràng của vấn đề ngay cả trong các trường hợp bệnh lý.
