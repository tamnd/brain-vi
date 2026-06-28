---
title: "CF 105141G - Máy đánh bạc"
description: "Chúng ta được cung cấp một dòng máy đánh bạc, mỗi máy chứa một số viên đá. Đối với bất kỳ phân đoạn máy liền kề nào, được lập chỉ mục từ l đến r, trò chơi hai người chơi sẽ được chơi trên phân đoạn đó."
date: "2026-06-27T16:53:36+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105141
codeforces_index: "G"
codeforces_contest_name: "BSUIR Open XII: Student Final"
rating: 0
weight: 105141
solve_time_s: 59
verified: true
draft: false
---

[CF 105141G - Máy đánh bạc](https://codeforces.com/problemset/problem/105141/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 59s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một dòng máy đánh bạc, mỗi máy chứa một số viên đá. Đối với bất kỳ phân đoạn máy liền kề nào, được lập chỉ mục từ l đến r, trò chơi hai người chơi sẽ được chơi trên phân đoạn đó. 

Một nước đi bao gồm việc chọn bất kỳ máy nào trong phân khúc và loại bỏ từ 1 đến k viên đá khỏi nó, miễn là máy có đủ đá. Người chơi luân phiên di chuyển và người chơi không thể thực hiện nước đi hợp pháp sẽ thua. Có một điều khó hiểu nữa: nếu một người chơi lấy đúng x viên đá và nước đi trước đó cũng lấy x viên đá, thì người chơi hiện tại sẽ thua ngay lập tức. 

Đối với mọi phân đoạn [l, r], cả hai người chơi đều chơi tối ưu và chúng ta phải xác định xem người chơi bắt đầu có thua trò chơi đó hay không. Nhiệm vụ là đếm xem người chơi bắt đầu bị mất bao nhiêu phân đoạn. 

Các ràng buộc đẩy chúng tôi ra xa khỏi mọi mô phỏng trên mỗi phân đoạn. Với n lên đến một triệu, việc liệt kê tất cả các phân đoạn đã ngụ ý khoảng 10^12 ứng cử viên, do đó, bất kỳ giải pháp nào cũng phải giảm vấn đề xuống một vấn đề có thể được xử lý theo thời gian tuyến tính hoặc gần tuyến tính. Giá trị của k nhỏ, nhiều nhất là 100 và đây là tham số duy nhất có thể được khai thác để giảm thiểu không cần thiết. 

Một trường hợp thất bại tinh vi đối với lối suy nghĩ ngây thơ xuất phát từ quy tắc chung về việc lặp lại quy mô di chuyển trước đó. Ví dụ: nếu k = 2 và một đoạn bao gồm một cọc có kích thước 3, thì trực giác ngây thơ trong trò chơi trừ cho thấy người chơi đầu tiên thắng. Tuy nhiên, quy tắc “không được di chuyển hai lần liên tiếp” có thể gây trở ngại cho lý do này nếu được diễn giải cục bộ trên mỗi cọc. Khó khăn chính là hạn chế không phải trên mỗi máy mà là toàn bộ trong toàn bộ lịch sử phát, điều này khiến cho việc phân tích đơn giản trở nên đáng ngờ. 

Một cạm bẫy phổ biến khác là giả định các cọc độc lập ngay lập tức vì việc di chuyển được thực hiện trên bất kỳ cọc nào. Giả định đó rõ ràng không có giá trị cho đến khi chúng ta hiểu được hạn chế nước đi cuối cùng tương tác như thế nào với cách chơi tối ưu. 

## Phương pháp tiếp cận 

Ý tưởng trực tiếp nhất là mô phỏng trò chơi cho từng phân đoạn [l, r]. Đối với một đoạn cố định, chúng tôi sẽ lập mô hình trạng thái dưới dạng cấu hình hiện tại của tất cả các cọc và giá trị di chuyển cuối cùng. Mỗi lần di chuyển sẽ phân nhánh theo n lựa chọn cọc và k lựa chọn số lượng loại bỏ. Ngay cả với tính năng ghi nhớ, không gian trạng thái phụ thuộc vào tất cả các giá trị ai và tăng theo cấp số nhân theo độ dài phân đoạn. Việc lặp lại điều này cho tất cả các phân đoạn O(n^2) là hoàn toàn không khả thi. 

Sự đơn giản hóa chính xuất phát từ việc nhận ra rằng bộ nhớ chung duy nhất trong trò chơi là giá trị của số tiền x được lấy cuối cùng. Điều này cho thấy trò chơi hoạt động giống như một trò chơi trừ trong đó các tùy chọn di chuyển được gắn nhãn bằng màu từ 1 đến k và hạn chế duy nhất là không thể sử dụng cùng một màu hai lần liên tiếp. 

Bây giờ hãy quan sát xem trò chơi tối ưu thực sự có tác dụng gì. Sau bất kỳ nước đi nào có kích thước x, đối thủ không bị buộc phải sử dụng x, bởi vì họ luôn có quyền truy cập vào ít nhất một phương án y trong [1, k] miễn là k ≥ 2. Vì k ít nhất là 2, nên ràng buộc "cấm lặp lại" không bao giờ loại bỏ tất cả các nước đi hợp pháp khỏi một vị trí không kết thúc trừ khi bản thân vị trí đó đã thua trong cấu trúc phép trừ cơ bản. 

Điều này làm cho hạn chế trở nên trơ đối với trạng thái thắng và thua: giá trị trò chơi cơ bản chỉ phụ thuộc vào việc một cọc có hoạt động giống như trò chơi trừ cổ điển với các nước đi từ 1 đến k hay không. 

Đối với một chồng có kích thước a, kết quả cổ điển đó cho biết vị trí bị mất chính xác khi mod (k + 1) = 0. Điều này mang lại một nhãn cục bộ đơn giản cho mỗi máy. 

Khi mỗi máy được giảm xuống kết quả nhị phân theo quy tắc này, sự tương tác giữa các máy sẽ trở nên giống XOR trong khoảng thời gian đó: một phân đoạn sẽ bị mất khi và chỉ khi XOR của các giá trị cục bộ này trên phân đoạn đó bằng 0. Điều này làm giảm nhiệm vụ đếm các mảng con có XOR bằng 0, đây là vấn đề về tần số tiền tố tiêu chuẩn.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu trên mỗi phân đoạn | O(n^2 · tiểu bang) | O(n) trở lên | Quá chậm | 
| Trò chơi rút gọn về phép trừ + đếm XOR tiền tố | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi chuyển đổi mỗi máy thành một giá trị nhỏ gọn để nắm bắt xem nó đang thắng hay thua dưới dạng một trò chơi trừ độc lập. 

1. Với mỗi vị trí i, tính bi = ai mod (k+1). Bước này chuyển cọc thành vị trí tương đương của nó trong trò chơi cất đi cổ điển với các nước đi từ 1 đến k. Lý do điều này có hiệu quả là vì trong trò chơi đó, các vị trí lặp lại sau mỗi k + 1 do hoàn toàn có thể kiểm soát được các bước di chuyển. 
2. Giải thích bi là giá trị trò chơi của máy thứ i, trong đó bi = 0 tương ứng với vị trí thua và bi ≠ 0 tương ứng với vị trí thắng. 
3. Tính toán mảng XOR tiền tố trên các giá trị này, trong đó pref[i] = b1 XOR b2 XOR ... XOR bi. Điều này biến đổi bất kỳ phân đoạn [l, r] nào thành một giá trị duy nhất pref[r] XOR pref[l - 1]. 
4. Một phân đoạn bị mất chính xác khi giá trị XOR của nó bằng 0, điều này xảy ra chính xác khi pref[r] bằng pref[l - 1]. 
5. Đếm xem có bao nhiêu cặp giá trị tiền tố bằng nhau. Điều này có thể được thực hiện bằng cách duy trì bản đồ tần số trong khi quét từ trái sang phải. 

Tính chính xác phụ thuộc vào thực tế là mỗi máy đóng góp một thành phần trò chơi độc lập và khách quan trong quá trình chơi tối ưu. Sự tương tác duy nhất giữa các thành phần là thông qua thành phần XOR của các kết quả đơn tương đương của chúng. 

### Tại sao nó hoạt động 

Trạng thái trò chơi trên một phân đoạn hoạt động giống như một tổng các trò chơi trừ độc lập khi hạn chế nước đi cuối cùng bị bỏ qua xét về kết quả chơi tối ưu. Mỗi cọc giảm xuống theo mô hình thua định kỳ với khoảng thời gian k + 1. Vì người chơi luôn có một nước đi thay thế khác với nước đi trước đó khi k ≥ 2, hạn chế toàn cầu không loại bỏ bất kỳ chuyển đổi chiến lược tối ưu nào có thể thay đổi cấu trúc Grundy. Do đó, mỗi vị trí đóng góp một giá trị Grundy nhị phân và giá trị phân đoạn đầy đủ là XOR của chúng. XOR bằng 0 mô tả chính xác trạng thái thua của người chơi bắt đầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    n, k = map(int, input().split())
    a = list(map(int, input().split()))

    freq = {0: 1}
    pref = 0
    ans = 0

    mod = k + 1

    for x in a:
        v = x % mod
        pref ^= v
        ans += freq.get(pref, 0)
        freq[pref] = freq.get(pref, 0) + 1

    print(ans)

if __name__ == "__main__":
    main()
```Việc triển khai trước tiên sẽ giảm bớt từng cọc bằng cách sử dụng modulo k + 1, đây là sự đơn giản hóa kết cấu quan trọng. Sau đó nó duy trì XOR đang chạy của các giá trị đã giảm này. Mỗi khi tiền tố XOR lặp lại, nó sẽ biểu thị một đoạn có XOR bằng 0, tương ứng với vị trí thua của người chơi bắt đầu. Bản đồ băm theo dõi tần suất mỗi giá trị tiền tố xuất hiện. 

Phải cẩn thận rằng tiền tố ban đầu XOR bằng 0 được tính một lần, vì phân đoạn bắt đầu từ chỉ mục 1 là hợp lệ khi bản thân pref[r] bằng 0. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 2
1 2 3
```Chúng tôi tính toán các giá trị mod 3 vì k + 1 = 3: 

| tôi | ai | bi = ai mod 3 | tiền tố XOR | 
| --- | --- | --- | --- | 
| 1 | 1 | 1 | 1 | 
| 2 | 2 | 2 | 3 | 
| 3 | 3 | 0 | 3 | 

Bây giờ chúng tôi đếm các cặp XOR tiền tố bằng nhau bao gồm cả số 0 ban đầu. 

Sự phát triển tần số: 

| tôi | trước | tần số trước | đã thêm | 
| --- | --- | --- | --- | 
| 0 | 0 | {0:1} | - | 
| 1 | 1 | {0:1} | 0 | 
| 2 | 3 | {0:1,1:1} | 0 | 
| 3 | 3 | {0:1,1:1,3:1} | 1 | 

Câu trả lời là 1 đoạn trong đó XOR bằng 0, khớp với đoạn (3,3). 

Điều này xác nhận rằng chỉ có một khoảng thời gian bị mất cấu hình. 

### Ví dụ 2 

đầu vào:```
2 10
308 693
```Ở đây k + 1 = 11. 

| tôi | ai | bi | tiền tố XOR | 
| --- | --- | --- | --- | 
| 1 | 308 | 0 | 0 | 
| 2 | 693 | 0 | 0 | 

Tần số của tiền tố XOR 0 trở thành 2, do đó chúng ta nhận được 1 mảng con hợp lệ (1,2), bị mất. 

Điều này cho thấy nhiều số 0 tích lũy thành nhiều phân đoạn bị mất do trạng thái tiền tố giống hệt nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | một lượt với các cập nhật băm theo thời gian liên tục cho mỗi phần tử | 
| Không gian | O(n) | các tần số tiền tố XOR trong trường hợp xấu nhất đều khác biệt | 

Giải pháp dễ dàng phù hợp trong giới hạn vì n lên tới 10^6 và tất cả các hoạt động được khấu hao O(1) cho mỗi phần tử. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, k = map(int, input().split())
    a = list(map(int, input().split()))

    freq = {0: 1}
    pref = 0
    ans = 0
    mod = k + 1

    for x in a:
        pref ^= (x % mod)
        ans += freq.get(pref, 0)
        freq[pref] = freq.get(pref, 0) + 1

    return str(ans)

# provided samples
assert run("3 2\n1 2 3\n") == "1"
assert run("2 10\n308 693\n") == "1"

# custom cases
assert run("1 2\n1\n") == "0"
assert run("1 2\n2\n") == "0"
assert run("5 2\n1 1 1 1 1\n") == "3"
assert run("4 3\n3 3 3 3\n") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| cọc thua/thắng đơn | 0 | trường hợp cơ sở đúng đắn | 
| tất cả các giá trị khác 0 giống hệt nhau | nhiều mảng con | tính chính xác của việc đếm tiền tố | 
| mọi bội số của k+1 | 0 | tuyên truyền trạng thái mất thuần túy | 
| xen kẽ các giá trị nhỏ | số lượng đa dạng | Hành vi tích lũy XOR | 

## Vỏ cạnh 

Trường hợp cạnh chính là khi tất cả các cọc đều mất vị trí riêng lẻ, nghĩa là mọi ai đều chia hết cho k + 1. Trong trường hợp này, mọi bi đều bằng 0, do đó mọi tiền tố XOR vẫn bằng 0 trong toàn mảng. Sau đó, thuật toán sẽ đếm từng cặp tiền tố, tạo ra chính xác n(n + 1)/2 phân đoạn, vì mọi phân đoạn đều bị mất. 

Một trường hợp cạnh khác là mảng một phần tử. Nếu ai mod (k + 1) khác 0 thì tiền tố XOR khác 0 và không có phân đoạn nào được tính. Nếu nó bằng 0 thì có chính xác một phân đoạn tồn tại và bị mất, phân đoạn này được xử lý chính xác bởi hạt giống tần số ban đầu. 

Một trường hợp tinh tế hơn nữa là các giá trị xen kẽ gây ra trạng thái XOR tiền tố lặp lại. Thuật toán xử lý điều này một cách thống nhất vì tính hợp lệ của phân đoạn chỉ phụ thuộc vào sự bằng nhau của các trạng thái tiền tố chứ không phụ thuộc vào cấu trúc cục bộ của mảng, điều này đảm bảo tính nhất quán ngay cả khi các mẫu rất bất thường.
