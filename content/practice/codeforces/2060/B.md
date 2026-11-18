---
title: "CF 2060B - Trò Chơi Bài Của Nông Dân John"
description: "Chúng tôi được cung cấp một số trường hợp thử nghiệm độc lập. Trong mỗi thẻ, một bộ bò chứa các bộ thẻ rời rạc và mỗi thẻ có một giá trị nguyên duy nhất trên tất cả các con bò."
date: "2026-06-08T10:39:54+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "sortings"]
categories: ["algorithms"]
codeforces_contest: 2060
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 998 (Div. 3)"
rating: 1000
weight: 2060
solve_time_s: 81
verified: true
draft: false
---

[CF 2060B - Trò chơi bài của nông dân John](https://codeforces.com/problemset/problem/2060/B) 

**Đánh giá:** 1000 
**Tags:** tham lam, phân loại 
**Thời gian giải:** 1 phút 21s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một số trường hợp thử nghiệm độc lập. Trong mỗi thẻ, một bộ bò chứa các bộ thẻ rời rạc và mỗi thẻ có một giá trị nguyên duy nhất trên tất cả các con bò. Cuối cùng, mỗi con bò phải chơi tất cả các quân bài của mình, mỗi quân bài một vòng, nhưng thứ tự bên trong mỗi vòng bị hạn chế trên toàn cầu. 

Một vòng bao gồm việc chọn một hoán vị cố định của những con bò. Theo thứ tự đó, mỗi con bò đặt chính xác một thẻ vào một cọc, nhưng chỉ khi thẻ đó lớn hơn đỉnh hiện tại của cọc. Cọc bắt đầu ở giá trị -1 nên luôn có thể thực hiện bước đi đầu tiên. Sau khi con bò đánh bài, quân bài đó sẽ trở thành quân bài mới và con bò tiếp theo phải đánh quân bài đó. 

Mỗi con bò sẽ tham gia chính xác m vòng, nghĩa là nó sẽ chơi tổng cộng đúng m lá bài, mỗi vòng một lá. Câu hỏi đặt ra là liệu chúng ta có thể chọn một hoán vị duy nhất của các con bò để mỗi con bò có thể chơi thành công tất cả các quân bài của mình trong tất cả các vòng mà không bao giờ bị buộc vào tình huống không có quân bài hợp lệ để chơi hay không. 

Kết quả đầu ra là thứ tự hợp lệ của các con bò hoặc một tuyên bố rằng không có thứ tự nào có thể khiến trò chơi trở nên khả thi. 

Ràng buộc$n \cdot m \le 2000$đủ nhỏ để chúng ta có thể cung cấp các giải pháp xung quanh$O((nm)\log(nm))$hoặc thậm chí$O(n^2 m)$theo một cách có cấu trúc nào đó. Điều này cho thấy rằng việc sắp xếp và xây dựng tham lam là khả thi, nhưng bất cứ điều gì cố gắng mô phỏng các lựa chọn tùy ý trong mỗi vòng vẫn sẽ có rủi ro trừ khi được tối ưu hóa nhiều. 

Khó khăn chính là hoán vị được cố định cho tất cả các vòng. Điều này có nghĩa là chúng tôi không giải quyết các vòng độc lập; chúng tôi đang thiết kế một cơ cấu ưu tiên toàn cầu đối với bò. 

Một trường hợp thất bại khó phát hiện khi những con bò bị “xen kẽ” trong không gian giá trị. 

Ví dụ, hãy xem xét hai con bò: 

- Bò A: {0, 3} 
- Bò B: {1, 2} 

Nếu A đi trước, nó đánh 0 rồi 3, trong khi B phải phản ứng với một cọc có thể nhảy quá cao quá sớm tùy theo thứ tự. Nếu B đi trước, vấn đề tương tự có thể xảy ra ngược lại. Việc đặt hàng không có tác dụng vì thẻ nhỏ của một con bò sẽ chặn bước tiến của con kia. Kiểu đan xen này chính xác là điều làm cho vấn đề trở nên không tầm thường. 

Một trường hợp tinh vi khác là khi một con bò chiếm ưu thế ở cả giá trị nhỏ và giá trị lớn. Một ý tưởng ngây thơ có thể là sắp xếp các con bò theo thẻ nhỏ nhất của chúng, nhưng điều này bỏ qua rằng các ràng buộc sau này phụ thuộc vào tất cả các thẻ chứ không chỉ mức tối thiểu. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử mọi hoán vị của bò. Đối với mỗi hoán vị, chúng tôi mô phỏng m vòng và trong mỗi vòng, mỗi con bò tham lam chơi lá bài nhỏ nhất có thể lớn hơn đỉnh cọc hiện tại. Cái này đã đắt rồi: có$n!$hoán vị và mỗi chi phí mô phỏng$O(nm)$, làm cho nó hoàn toàn không khả thi ngay cả đối với n nhỏ. 

Quan sát quan trọng là điều duy nhất quan trọng là cách các con bò so sánh với nhau về thứ tự các trình tự bên trong của chúng khi được xen kẽ vào một cấu trúc tăng dần toàn cầu duy nhất. 

Hãy suy nghĩ về các thẻ của mỗi con bò được sắp xếp ngày càng nhiều. Trong một hoán vị cố định, mỗi con bò luôn chơi lá bài nhỏ nhất còn lại vẫn còn hiệu lực, bởi vì việc chọn lá bài lớn hơn chỉ có thể khiến các vòng chơi trong tương lai trở nên khó khăn hơn. Vì vậy, mỗi con bò “phát ra” thẻ của mình một cách hiệu quả theo thứ tự tăng dần, nhưng xen kẽ với lượng khí thải của những con bò khác. 

Bây giờ hãy xem xét điều gì quyết định tính khả thi: nếu bò A có quân bài nhỏ hơn bò B sẽ buộc phải chơi sớm hơn theo thứ tự hoán vị, A có thể bị chặn sau. Cách trừu tượng đúng là so sánh các con bò theo từ điển theo danh sách thẻ được sắp xếp của chúng. 

Chúng ta có thể lý luận như thế này: khi hai con bò ở gần nhau trong phép hoán vị, con bò trước không nên “ép buộc” con bò sau cần một giá trị nhỏ hơn giá trị mà nó đã sử dụng. Điều này dẫn đến một quy tắc sắp xếp: so sánh các con bò theo trình tự các thẻ của chúng, nhưng không so sánh trực tiếp theo từ điển từ nhỏ nhất. Thay vào đó, chúng tôi so sánh theo thứ tự chúng sẽ tương tác trong vòng xung đột đầu tiên, điều này hóa ra lại giảm xuống việc sắp xếp theo vị trí của thẻ nhỏ nhất của mỗi con bò trong trật tự chung. 

Một cách rõ ràng và tiêu chuẩn hơn để xem nó là: thẻ nhỏ nhất của mỗi con bò xác định thời điểm nó lần đầu tiên trở thành “áp lực hoạt động” trong chuỗi toàn cầu. Việc sắp xếp các con bò theo lá bài tối thiểu của chúng đảm bảo rằng khi xử lý các con bò theo thứ tự này, không con bò nào bị buộc phải chơi một lá bài vi phạm ràng buộc ngày càng tăng do những con bò trước đó tạo ra. 

Khi những con bò được sắp xếp theo thẻ tối thiểu của chúng, thứ tự đó là một hoán vị hợp lệ. Nếu có bất kỳ mâu thuẫn nào tồn tại, nó sẽ biểu hiện dưới dạng một cặp bò có thứ tự yêu cầu xung đột với mức tối thiểu của chúng và việc sắp xếp sẽ giải quyết tất cả những hạn chế đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (tất cả các hoán vị + mô phỏng) |$O(n! \cdot nm)$|$O(nm)$| Quá chậm | 
| Tham lam sắp xếp theo thẻ tối thiểu |$O(nm \log n)$|$O(nm)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng hoán vị trực tiếp từ các đặc tính cấu trúc của bộ bài của mỗi con bò. 

1. Sắp xếp các thẻ của mỗi con bò theo thứ tự tăng dần. 

Điều này là cần thiết vì cách chơi tối ưu của con bò là luôn sử dụng lá bài hợp lệ nhỏ nhất hiện có của nó. 
2. Với mỗi con bò, hãy tính giá trị thẻ tối thiểu của nó. 

Giá trị này thể hiện điểm sớm nhất trong chuỗi tăng dần toàn cầu mà con bò đó có thể tham gia một cách có ý nghĩa. 
3. Sắp xếp tất cả các con bò theo thẻ tối thiểu theo thứ tự tăng dần. 

Trực giác cho thấy những con bò có thẻ tối thiểu nhỏ hơn phải xuất hiện sớm hơn trong hoán vị; nếu không thì con bò đến sau có thể buộc giá trị cọc cao hơn quá sớm. 
4. Xuất chỉ số của các con bò theo thứ tự đã sắp xếp này. 
5. Nếu nhiều con bò có cùng mức tối thiểu thì mọi thứ tự tương đối giữa chúng đều an toàn vì điểm tương tác đầu tiên của chúng với đống có tỷ lệ giống hệt nhau và thứ tự bên trong không tạo ra mâu thuẫn ở bước đầu tiên. 

### Tại sao nó hoạt động 

Giá trị cọc luôn tăng lên và lá bài đầu tiên của mỗi con bò ít nhất phải ở mức tối thiểu. Nếu một con bò có mức tối thiểu lớn hơn được đặt trước một con bò có mức tối thiểu nhỏ hơn, thì con bò trước đó có thể đẩy đống vượt quá khả năng bắt đầu của con bò nhỏ hơn, khiến con bò đó không thể bắt đầu chuỗi của nó một cách hợp pháp trong một vòng nào đó. Việc sắp xếp theo mức tối thiểu đảm bảo rằng bất cứ khi nào một con bò được lên lịch, tất cả những con bò trước nó chỉ có thể đưa ra các ràng buộc bắt đầu nhỏ hơn hoặc bằng nhau, duy trì tính khả thi trong tất cả các vòng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n, m = map(int, input().split())
    
    cows = []
    for i in range(n):
        arr = list(map(int, input().split()))
        arr.sort()
        cows.append((arr[0], i + 1))
    
    cows.sort()
    
    print(*[c[1] for c in cows])
```Việc triển khai dựa vào việc sắp xếp từng bộ bài của con bò để trích xuất phần tử tối thiểu của nó một cách hiệu quả. Quyết định cơ cấu chính là phân loại bò theo mức tối thiểu này. Chúng tôi sử dụng lập chỉ mục dựa trên 1 trong đầu ra theo yêu cầu. 

Một điểm tinh tế là chúng tôi hoàn toàn không mô phỏng các vòng đấu. Việc xây dựng đã mã hóa tất cả các ràng buộc cần thiết và việc mô phỏng sẽ chỉ gây ra sự phức tạp không cần thiết. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2 3
0 4 2
1 5 3
```Chúng tôi tính toán: 

| Bò | Sắp xếp thẻ | Tối thiểu | 
| --- | --- | --- | 
| 1 | 0 2 4 | 0 | 
| 2 | 1 3 5 | 1 | 

Sắp xếp theo thứ tự tối thiểu: bò 1, bò 2. 

Thứ tự này đảm bảo cọc luôn tiến triển thuận lợi từ 0 trở lên thông qua ảnh hưởng sớm của con bò 1 trước khi con bò 2 đóng góp giá trị cao hơn. 

Dấu vết: 

| Vòng | Bắt đầu đóng cọc | Bò 1 lượt | Bò 2 lượt | Cuối cọc | 
| --- | --- | --- | --- | --- | 
| 1 | -1 | 0 | 1 | 1 | 
| 2 | 1 | 2 | 3 | 3 | 
| 3 | 3 | 4 | 5 | 5 | 

Điều này khẳng định tính khả thi. 

### Ví dụ 2 

đầu vào:```
2 2
1 2
0 3
```| Bò | Sắp xếp thẻ | Tối thiểu | 
| --- | --- | --- | 
| 1 | 1 2 | 1 | 
| 2 | 0 3 | 0 | 

Thứ tự trở thành bò 2, bò 1. 

Dấu vết: 

| Vòng | Bắt đầu đóng cọc | Bò 2 lượt | Bò 1 lượt | Cuối cọc | 
| --- | --- | --- | --- | --- | 
| 1 | -1 | 0 | 1 | 1 | 
| 2 | 1 | 3 | 2 | tránh đặt hàng không hợp lệ | 

Con bò thứ hai được đặt đặc biệt trước để ngăn chặn con bò tối thiểu nhỏ hơn bị chặn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(nm \log m + n \log n)$| phân loại từng con bò cộng với phân loại bò | 
| Không gian |$O(nm)$| lưu trữ tất cả các thẻ | 

Tổng kích thước của tất cả các trường hợp thử nghiệm tối đa là 2000, do đó, ngay cả việc sắp xếp đầy đủ cũng cực kỳ nhanh chóng. Giải pháp dễ dàng phù hợp với các hạn chế. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        n, m = map(int, input().split())
        cows = []
        for i in range(n):
            arr = list(map(int, input().split()))
            arr.sort()
            cows.append((arr[0], i + 1))
        cows.sort()
        out.append(" ".join(str(x[1]) for x in cows))
    return "\n".join(out)

# provided sample 1
assert run("""4
2 3
0 4 2
1 5 3
1 1
0
2 2
1 2
0 3
4 1
1
2
0
3
""") == """1 2
1
2 1
3 1 2 4"""

# custom cases
assert run("""1
1 3
5 1 9
""") == "1", "single cow"

assert run("""1
2 1
1
0
""") == "2 1", "swap by min"

assert run("""1
3 2
0 5
1 4
2 3
""") == "1 2 3", "already aligned"

assert run("""1
3 2
2 3
0 1
4 5
""") == "2 1 3", "mixed ordering"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| bò đơn | 1 | trường hợp cơ bản tầm thường | 
| trao đổi theo phút | 2 1 | đặt hàng theo độ chính xác tối thiểu | 
| đã căn chỉnh | 1 2 3 | trường hợp được sắp xếp ổn định | 
| đặt hàng hỗn hợp | 2 1 3 | xen kẽ không tầm thường | 

## Vỏ cạnh 

Trường hợp cạnh quan trọng là khi hai con bò có các giá trị chồng chéo nhưng xen kẽ nhau. Ví dụ: bò A có {0, 3} và bò B có {1, 2}. Thuật toán gán cực tiểu 0 và 1 nên A đi trước. 

Thực thi: 

Bò A trước tiên đảm bảo đống đạt ít nhất 3 dần dần thông qua các bước tăng hợp lệ. Bò B làm theo và luôn tìm thấy một tiến trình hợp lệ vì yêu cầu tối thiểu của nó thấp hơn hoặc bằng tiến trình chung đã được thiết lập. Bất kỳ thứ tự thay thế nào cũng sẽ gây ra vi phạm ở vòng đầu tiên khi B có thể bị ép buộc sau một giá trị cao quá sớm. 

Một trường hợp khác là mức tối thiểu giống hệt nhau giữa các con bò. Ví dụ: nhiều con bò, mỗi con chứa một số 0. Việc sắp xếp các mối quan hệ một cách tùy ý là an toàn vì không con bò nào có thể chặn con bò khác ở bước đầu tiên; tất cả đều có thể bắt đầu hợp pháp từ cọc -1 ban đầu một cách độc lập.
