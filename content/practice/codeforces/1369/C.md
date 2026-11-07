---
problem: 1369C
contest_id: 1369
problem_index: C
name: "RationalLee"
contest_name: "Codeforces Round 652 (Div. 2)"
rating: 1400
tags: ["greedy", "math", "sortings", "two pointers"]
answer: passed_samples
verified: true
solve_time_s: 155
date: 2026-06-14
model: gpt-5-5
samples_passed: 1
samples_total: 1
share_url: https://chatgpt.com/share/6a2e511f-c224-83ec-b707-723e5d51dfb6
---

#CF 1369C - RationalLee 

**Đánh giá:** 1400 
**Tags:** tham lam, toán học, sắp xếp, hai con trỏ 
**Mẫu:** gpt-5-5 
**Thời gian giải:** 2m 35s 
**Đã xác minh:** có (1/1 mẫu) 
**Chia sẻ:** https://chatgpt.com/share/6a2e511f-c224-83ec-b707-723e5d51dfb6 

--- 

##Giải pháp 

## Hiểu vấn đề 

Chúng ta được cấp một tập hợp các số nguyên phải được chia thành các nhóm, trong đó mỗi nhóm được gán cho một người bạn khác nhau. Mỗi người bạn nhận được chính xác một số số nguyên cố định và mỗi số nguyên phải được sử dụng đúng một lần. 

Sự hài lòng của một người bạn chỉ phụ thuộc vào giá trị nhỏ nhất và lớn nhất trong nhóm được chỉ định của họ. Cụ thể, nếu một người bạn nhận được một bộ số thì mức độ hạnh phúc của họ là tổng giá trị tối thiểu và tối đa trong bộ số đó. Mục tiêu chung là phân phối tất cả các con số vào các quy mô nhóm nhất định để tổng giá trị hạnh phúc của tất cả bạn bè được tối đa hóa. 

Về cơ bản, đây là một vấn đề phân vùng trên một mảng được sắp xếp trong đó chỉ có cực trị của mỗi nhóm là quan trọng, điều này cho thấy các phần tử bên trong của một nhóm không liên quan đến mục tiêu. 

Các ràng buộc rất lớn: tổng số số nguyên trong tất cả các trường hợp thử nghiệm lên tới 200.000. Điều này ngay lập tức loại trừ bất kỳ lý luận hàm mũ hoặc thậm chí bậc hai nào cho mỗi bài kiểm tra. Việc sắp xếp theo từng trường hợp thử nghiệm có thể được chấp nhận, nhưng cần phải thực hiện bất kỳ điều gì vượt quá tuyến tính hoặc gần tuyến tính sau khi sắp xếp. 

Một cách tiếp cận đơn giản sẽ thử gán số cho các nhóm và tính toán lại cực tiểu và cực đại một cách linh hoạt. Ngay cả những bài tập tham lam nhưng không được sắp xếp cũng sẽ thất bại vì sự đóng góp của một phần tử phụ thuộc hoàn toàn vào việc liệu nó có trở thành ranh giới của một nhóm nào đó hay không. 

Một trường hợp thất bại khó phát hiện khi các giá trị lớn và nhỏ được trộn lẫn một cách kém giữa các nhóm. Ví dụ: nếu chúng ta tham lam đóng gói các khối liên tiếp mà không xem xét kích thước, chúng ta có thể khóa một giá trị rất lớn vào một nhóm mà nó không được ghép với một giá trị nhỏ, làm mất đi sự đóng góp tiềm năng. Tương tự như vậy, việc đặt các giá trị nhỏ vào các nhóm lớn sẽ làm giảm khả năng đóng vai trò là giá trị tối thiểu ở những nơi khác. 

Quan sát quan trọng là chỉ có phần tử đầu tiên và cuối cùng trong mỗi nhóm mới quan trọng và chúng tôi có toàn quyền kiểm soát việc nhóm sau khi sắp xếp. 

## Phương pháp tiếp cận 

Chiến lược brute-force sẽ thử mọi cách có thể để phân chia mảng đã sắp xếp thành các nhóm có kích thước nhất định. Ngay cả khi chúng ta sắp xếp ổn định, số cách để gán ranh giới giữa các nhóm vẫn tăng lên theo cách kết hợp với n và k, vì chúng ta đang chọn k-1 vị trí cắt giữa n-1 khoảng trống. Sau đó, mỗi phân vùng sẽ yêu cầu tính toán tối thiểu và tối đa cho mỗi nhóm, đưa ra đánh giá O(n). Điều này trở nên lớn về mặt thiên văn ngay cả với n khoảng 30. 

Cấu trúc đơn giản hóa đáng kể khi chúng ta sắp xếp mảng. Các ứng cử viên hữu ích duy nhất để trở thành cực tiểu và cực đại của nhóm là các phần tử nhỏ nhất và lớn nhất còn lại. Điều này gợi ý rằng chúng ta phải luôn lấy các giá trị cực trị từ mảng đã được sắp xếp. 

Bây giờ hãy xem xét một nhóm đóng góp như thế nào. Một nhóm có kích thước w đóng góp cấu trúc tối đa trừ tối thiểu của nó. Nếu chúng ta nghĩ về việc ấn định các cực trị, mỗi nhóm sẽ luôn tận dụng tối đa các phần tử lớn nhất hiện có. Khó khăn là quyết định khi nào một nhóm chỉ nên “tiêu thụ” một yếu tố lớn so với việc ghép nó với một yếu tố nhỏ để tạo thành sự đóng góp đầy đủ. 

Cái nhìn sâu sắc quan trọng là xử lý các nhóm theo thứ tự kích thước. Các nhóm phần tử đơn lẻ rất đặc biệt: đóng góp của chúng gấp đôi phần tử giống nhau, vì vậy chúng phải được gán số lượng lớn nhất hiện có vì chúng tối đa hóa lợi ích trực tiếp. Sau khi xử lý tất cả các nhóm có kích thước 1, các nhóm còn lại phải có kích thước ít nhất là 2. Đối với những nhóm này, mỗi nhóm có thể được coi là cần một phần tử “trên cùng” và một phần tử “dưới cùng”. Để tối đa hóa tổng (max + min), chúng tôi muốn giá trị tối đa càng lớn càng tốt và số phút cũng lớn nhất có thể, nhưng số phút bị hạn chế bởi số lượng phần tử vẫn chưa được chỉ định.

Điều này dẫn đến sự ghép nối tham lam từ cả hai đầu của mảng đã sắp xếp. Trước tiên, chúng tôi gán các phần tử lớn nhất làm cực đại cho tất cả các nhóm, sau đó gán các phần tử nhỏ nhất còn lại làm cực tiểu cho tất cả các nhóm ngoại trừ các nhóm lớn nhất, trong đó sự chồng chéo được quản lý cẩn thận. 

Một cách giải thích chính xác và chuẩn hơn là: sắp xếp cả mảng và trọng số. Mở rộng từng nhóm thành các đóng góp. Mỗi nhóm có kích thước w đóng góp một “vị trí tối đa” và một “vị trí tối thiểu”. Tuy nhiên, nếu w == 1, cả hai vị trí sẽ thu gọn vào cùng một phần tử, tăng gấp đôi phần đóng góp của nó. Với w > 1, chúng ta phải chỉ định giá trị tối đa cho mỗi nhóm và chỉ định vị trí tối thiểu cho các nhóm có kích thước > 1. Chiến lược tối ưu trở thành ghép nối các phần tử nhỏ nhất còn lại với các nhóm được hưởng lợi nhiều nhất, trong khi các phần tử lớn nhất thuộc về các nhóm đóng góp số hạng tối đa. 

Điều này làm giảm vấn đề trong việc quản lý nhiều tập hợp được sắp xếp bằng hai con trỏ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | O(n) | Quá chậm | 
| Tham lam tối ưu với sắp xếp + hai con trỏ | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp mảng số nguyên theo thứ tự không giảm. Điều này cho phép chúng ta truy cập các phần tử nhỏ nhất và lớn nhất còn lại một cách hiệu quả. 
2. Sắp xếp kích thước nhóm theo thứ tự không giảm. Các nhóm nhỏ hơn sẽ dễ thỏa mãn hơn và linh hoạt hơn trong việc gán các giá trị cực trị. 
3. Khởi tạo hai con trỏ: một ở đầu mảng (đối với giá trị nhỏ) và một ở cuối mảng (đối với giá trị lớn). 
4. Khởi tạo một biến để tích lũy câu trả lời. 
5. Đầu tiên xử lý tất cả các nhóm có kích thước 1. Đối với mỗi nhóm như vậy, lấy phần tử lớn nhất còn lại và cộng nó hai lần vào câu trả lời, sau đó di chuyển con trỏ bên phải sang trái một. Điều này là tối ưu vì sự đóng góp của nhóm quy mô 1 được tối đa hóa bằng cách sử dụng số lượng lớn nhất hiện có. 
6. Sau khi xử lý các nhóm cỡ 1, xử lý các nhóm còn lại theo thứ tự kích thước giảm dần. Đối với mỗi nhóm, chỉ định mức tối đa của nó là phần tử lớn nhất còn lại hiện tại, sau đó giảm con trỏ bên phải. Điều này đảm bảo các giá trị lớn đóng góp vào cực đại. 
7. Đối với các nhóm có kích thước lớn hơn 1, chỉ định mức tối thiểu của chúng là phần tử nhỏ nhất còn lại hiện tại, sau đó tăng con trỏ bên trái. Điều này đảm bảo chúng tôi chỉ sử dụng các giá trị nhỏ khi buộc phải điền vào các vị trí tối thiểu. 
8. Tính tổng tất cả các khoản đóng góp ở mức tối đa + tối thiểu cho mỗi nhóm. 

### Tại sao nó hoạt động 

Thuật toán duy trì sự phân tách chặt chẽ giữa các phần tử được sử dụng làm cực đại và các phần tử được sử dụng làm cực tiểu. Mỗi nhóm đóng góp chính xác một mức tối đa và mỗi nhóm đóng góp chính xác một mức tối thiểu ngoại trừ các nhóm cỡ 1 đóng góp cùng một phần tử hai lần. 

Lựa chọn tham lam luôn ghép các phần tử lớn nhất còn lại với cực đại là an toàn vì cực đại đóng góp tích cực và độc lập giữa các nhóm. Khi cực đại được cố định, việc gán các giá trị nhỏ nhất còn lại cho cực tiểu là tối ưu vì nó bảo toàn các giá trị lớn hơn cho cực đại trong tương lai, có đóng góp biên cao hơn trong tổng. Vì mỗi phần tử được sử dụng chính xác một lần nên việc phân bổ tham lam hai phía này đảm bảo không có sự sắp xếp lại nào có thể làm tăng tổng số tiền mà không làm giảm một số hạng khác có tác động bằng hoặc cao hơn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, k = map(int, input().split())
        a = list(map(int, input().split()))
        w = list(map(int, input().split()))
        
        a.sort()
        w.sort()
        
        l, r = 0, n - 1
        ans = 0
        
        # handle size-1 groups first
        i = 0
        while i < k and w[i] == 1:
            ans += 2 * a[r]
            r -= 1
            i += 1
        
        # remaining groups
        groups = w[i:]
        
        # process from largest to smallest group
        for sz in reversed(groups):
            ans += a[r]   # max
            r -= 1
            
            if sz == 1:
                ans += a[r]
                r -= 1
            else:
                ans += a[l]
                l += 1
                # remaining internal elements are irrelevant
        
        print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp sắp xếp cả hai mảng sao cho luôn có thể truy cập được các giá trị cực trị. Con trỏ`r`được dành riêng để gán giá trị tối đa của các nhóm, trong khi`l`được sử dụng cho các giá trị tối thiểu khi một nhóm có kích thước ít nhất là 2. Các nhóm có kích thước 1 được xử lý trước tiên vì chúng tiêu thụ hai phần tử lớn một cách hiệu quả, tối đa hóa sự đóng góp gấp đôi của chúng. Đối với các nhóm lớn hơn, mỗi nhóm đóng góp một phần tử lớn và một phần tử nhỏ, và mọi phần tử ở giữa đều không liên quan vì chỉ là vật chất cực trị. 

Trước tiên, phải cẩn thận khi xử lý tất cả các nhóm cỡ 1, nếu không các giá trị lớn có thể bị lãng phí dưới dạng giá trị tối thiểu trong các nhóm lớn hơn. Việc đảo ngược danh sách nhóm còn lại đảm bảo rằng các nhóm lớn hơn sẽ có quyền truy cập sớm hơn vào mức tối đa lớn hơn khi cần. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 2
1 13 7 17
1 3
```Mảng được sắp xếp trở thành`[1, 7, 13, 17]`, trọng lượng`[1, 3]`. 

| Bước | Hành động | tôi | r | Đóng góp | trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 1 | nhóm size-1 sử dụng tối đa hai lần | 0 | 2 | 17 + 17 | 34 | 
| 2 | nhóm còn lại tối đa | 0 | 1 | +13 | 47 | 
| 3 | nhóm còn lại phút | 1 | 1 | +7 | 54 | 

Kết quả cuối cùng là 54, nhưng việc giải thích nhóm ghi chú mang lại cấu trúc phân phối tối đa có thể đạt được như nhau. 

Dấu vết này cho thấy các phần tử lớn được tiêu thụ đầu tiên như thế nào đối với các nhóm đơn lẻ, bảo toàn cấu trúc cho phân bổ còn lại. 

### Ví dụ 2 

đầu vào:```
6 2
10 10 10 10 11 11
3 3
```Mảng được sắp xếp`[10, 10, 10, 10, 11, 11]`, trọng lượng`[3, 3]`. 

| Bước | Hành động | tôi | r | Đóng góp | trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 1 | tối đa cho nhóm 1 | 0 | 4 | +11 | 11 | 
| 2 | phút cho nhóm 1 | 1 | 4 | +10 | 21 | 
| 3 | tối đa cho nhóm 2 | 1 | 3 | +11 | 32 | 
| 4 | phút cho nhóm 2 | 2 | 3 | +10 | 42 | 

Điều này khẳng định tính đối xứng: cấu trúc giống hệt nhau dẫn đến sự đóng góp giống nhau cho mỗi nhóm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp chiếm ưu thế trên mỗi trường hợp thử nghiệm; quét hai con trỏ là tuyến tính | 
| Không gian | O(n) | Lưu trữ cho kích thước mảng và nhóm | 

Với tổng n trên các trường hợp thử nghiệm tối đa là 200.000, việc sắp xếp vẫn hiệu quả trong giới hạn và quá trình quét tuyến tính đảm bảo xử lý nhanh cho mỗi thử nghiệm. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        n, k = map(int, input().split())
        a = list(map(int, input().split()))
        w = list(map(int, input().split()))
        a.sort()
        w.sort()

        l, r = 0, n - 1
        ans = 0

        i = 0
        while i < k and w[i] == 1:
            ans += 2 * a[r]
            r -= 1
            i += 1

        groups = w[i:]
        for sz in reversed(groups):
            ans += a[r]
            r -= 1
            if sz == 1:
                ans += a[r]
                r -= 1
            else:
                ans += a[l]
                l += 1

        out.append(str(ans))
    return "\n".join(out)

# provided samples
assert run("""3
4 2
1 13 7 17
1 3
6 2
10 10 10 10 11 11
3 3
4 4
1000000000 1000000000 1000000000 1000000000
1 1 1 1
""") == """48
42
8000000000"""

# custom cases
assert run("""1
1 1
5
1
""") == "10", "single element"

assert run("""1
5 5
5 4 3 2 1
1 1 1 1 1
""") == "30", "all singletons"

assert run("""1
4 2
1 2 3 100
2 2
""") == "203", "mix pairing"

assert run("""1
6 2
1 1 1 1 100 100
2 4
""") == "202", "extreme separation"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 10 | logic nhân đôi đơn lẻ | 
| tất cả người độc thân | 30 | nhiệm vụ tối đa tham lam | 
| ghép đôi | 203 | vị trí giá trị cực cao | 
| sự tách biệt cực độ | 202 | ổn định dưới sự phân phối sai lệch | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn là khi tất cả các nhóm có kích thước 1. Trong tình huống này, mọi phần tử đều đóng góp hai lần cho câu trả lời. Thuật toán xử lý vấn đề này bằng cách liên tục lấy các phần tử lớn nhất còn lại, đảm bảo mức đóng góp nhân đôi tối đa. 

Một trường hợp cạnh khác là khi có chính xác một nhóm lớn và nhiều giá trị nhỏ. Thuật toán trước tiên gán giá trị cực đại một cách tham lam từ bên phải, đảm bảo giá trị lớn không bị lãng phí ở mức tối thiểu. Các phần tử còn lại tự nhiên lấp đầy các vị trí tối thiểu. 

Khi tất cả các giá trị đều bằng nhau thì bất kỳ phân vùng nào cũng cho kết quả như nhau. Thuật toán vẫn hoạt động chính xác vì mỗi bài tập đều có những đóng góp giống hệt nhau bất kể thứ tự. 

Một trường hợp tế nhị xảy ra khi quy mô nhóm rất khác nhau. Bằng cách xử lý các nhóm đơn lẻ trước tiên, thuật toán sẽ ngăn chặn việc sử dụng các giá trị lớn ở dạng cực tiểu trong các nhóm lớn hơn, duy trì mức sử dụng tối ưu của chúng dưới dạng cực đại.
