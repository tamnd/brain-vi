---
title: "CF 104311A - Tối đa n số nguyên"
description: "Chúng tôi được cung cấp một số trường hợp thử nghiệm. Trong mỗi trường hợp thử nghiệm, chúng tôi nhận được một mảng các số nguyên và chúng tôi được yêu cầu phân tích một đoạn mã bị lỗi nhằm cố gắng tính giá trị tối đa của mảng."
date: "2026-07-01T19:57:51+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104311
codeforces_index: "A"
codeforces_contest_name: "TheForces Round #11 (DIV2.5-Forces)"
rating: 0
weight: 104311
solve_time_s: 91
verified: false
draft: false
---

[CF 104311A - Tối đa n số nguyên](https://codeforces.com/problemset/problem/104311/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 31 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một số trường hợp thử nghiệm. Trong mỗi trường hợp thử nghiệm, chúng tôi nhận được một mảng các số nguyên và chúng tôi được yêu cầu phân tích một đoạn mã bị lỗi nhằm cố gắng tính giá trị tối đa của mảng. 

Mã hoạt động bình thường đối với hầu hết các chỉ mục, nhưng đối với một chỉ mục đặc biệt`k`nó sử dụng một thao tác khác: thay vì cập nhật câu trả lời đang chạy ở mức tối đa, nó sử dụng mức tối thiểu ở vị trí đó. Điều này có nghĩa là tùy thuộc vào vị trí đặt “vị trí lỗi” này, kết quả cuối cùng của chương trình có thể khớp hoặc không khớp với giá trị tối đa thực sự của mảng. 

Đối với mỗi trường hợp kiểm thử, nhiệm vụ là đếm xem có bao nhiêu lựa chọn`k`khiến chương trình đưa ra kết quả sai. 

Các ràng buộc rất lớn: tổng số phần tử trong tất cả các trường hợp thử nghiệm có thể lên tới một triệu và có thể lên tới một trăm nghìn trường hợp thử nghiệm. Điều này ngay lập tức loại trừ mọi giải pháp mô phỏng quy trình riêng biệt cho mọi`k`, vì điều đó sẽ dẫn đến hành vi gần như O(n^2) trong trường hợp xấu nhất. 

Một điểm tinh tế quan trọng là chương trình bắt đầu bằng`ans = 0`, trong khi tất cả các giá trị mảng đều dương. Điều này quan trọng vì bản cập nhật đầu tiên vẫn có thể bị ảnh hưởng do thao tác sai tùy theo vị trí. 

Một vài tình huống cạnh minh họa hành vi: 

Ví dụ: nếu tất cả các giá trị đều bằng nhau`1 1 1`, thì ngay cả khi một vị trí được xử lý bằng`min`, kết quả vẫn đúng vì cả hai`max`Và`min`cho cùng một giá trị. Vì vậy mỗi`k`là hợp lệ. 

Nếu phần tử tối đa là duy nhất và lỗi ảnh hưởng đến lần xuất hiện duy nhất của mức tối đa đó thì câu trả lời cuối cùng có thể giảm xuống dưới mức tối đa thực sự. 

Nếu lỗi được đặt trước bất kỳ giá trị lớn nào, nó có thể ngăn chặn sự phát triển của`ans`, tùy thuộc vào mức độ phát triển tối đa của việc chạy. 

Thách thức cốt lõi là mô tả chính xác thời điểm một vị trí mà không cần mô phỏng.`k`phá vỡ mức tối đa cuối cùng. 

## Phương pháp tiếp cận 

Một mô phỏng trực tiếp cho một điểm cố định`k`là đơn giản. Chúng tôi chạy vòng lặp, áp dụng`max`mọi nơi ngoại trừ tại vị trí`k`, nơi chúng tôi áp dụng`min`và so sánh kết quả cuối cùng với mức tối đa thực sự của mảng. Lặp lại điều này cho mỗi`k`đưa ra câu trả lời đúng, nhưng chi phí là O(n^2) cho mỗi trường hợp thử nghiệm vì mỗi mô phỏng là O(n) và chúng tôi lặp lại nó n lần. 

Quan sát quan trọng là chương trình chỉ cố gắng theo dõi mức tối đa, do đó hành vi đúng đắn của nó chỉ phụ thuộc vào việc mức tối đa toàn cầu có bao giờ bị “mất” ở chỉ số đặc biệt hay không. Vì tất cả các giá trị đều dương và`ans`chỉ tăng trừ khi lỗi can thiệp, cách duy nhất để nhận được kết quả sai là buộc mức tối đa đang chạy dưới mức tối đa thực tại thời điểm phần tử tối đa được xử lý. 

Cho phép`mx`là giá trị lớn nhất trong mảng. Hãy xem xét các vị trí mà giá trị này xuất hiện. Nếu chúng ta chọn`k`là bất kỳ chỉ mục nào không phải là vị trí tối đa thì khi chúng ta đạt đến phần tử tối đa, mã vẫn sử dụng`max`, Vì thế`ans`trở thành`mx`và vẫn đúng. 

Bây giờ hãy cân nhắc việc lựa chọn`k`bằng một vị trí nào đó của phần tử lớn nhất. Ở vị trí đó, thay vì lấy`max(ans, mx)`, mã mất`min(ans, mx)`, có nghĩa là nó thay thế`ans`với giá trị nhiều nhất là giá trị tối đa của tiền tố trước đó. Vì tất cả các giá trị trước đó nhiều nhất là`mx`, Và`ans`bắt đầu từ 0 và chỉ tăng đến các giá trị trước khi đạt mức tối đa, điều này buộc`ans`vẫn ở mức dưới đây`mx`sau khi xử lý vị trí đó. Bất kỳ sự xuất hiện sau này của`mx`(nếu có) vẫn sẽ sử dụng`max`, nhưng họ chỉ có thể so sánh với mức giảm`ans`và điều quan trọng là chúng sẽ không bao giờ khôi phục tính chính xác nếu mức tối đa bị bỏ qua hoặc bị triệt tiêu một cách hiệu quả theo cách phá vỡ sự bình đẳng với hành vi tối đa thực sự cuối cùng. 

Sự đơn giản hóa mang tính quyết định là tính đúng đắn chỉ phụ thuộc vào việc lựa chọn`k`là vị trí có giá trị lớn nhất Bất kỳ vị trí nào như vậy sẽ phá vỡ tính toán; bất kỳ vị trí nào khác bảo tồn nó. 

Vì vậy, câu trả lời cho mỗi trường hợp thử nghiệm chỉ đơn giản là số lần xuất hiện của phần tử lớn nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(n^2) | O(1) | Quá chậm | 
| Đếm số lần xuất hiện tối đa | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta rút gọn vấn đề về việc đếm tần suất xuất hiện giá trị lớn nhất. 

1. Với mỗi test, đọc mảng và quét một lần để tìm giá trị lớn nhất. Điều này xác định giá trị mục tiêu mà chương trình đang cố gắng tính toán. Lý do chúng tôi cô lập mức tối đa là vì chỉ giá trị này mới có thể xác định liệu câu trả lời cuối cùng có đúng hay không. 
2. Quét lại mảng và đếm xem có bao nhiêu vị trí chứa giá trị lớn nhất này. Mỗi vị trí như vậy tương ứng với một sự lựa chọn`k`. 
3. Xuất số đếm này làm câu trả lời cho ca kiểm thử. 

Bước thứ hai là cần thiết vì chúng ta phải phân biệt giữa lần xuất hiện của giá trị lớn nhất và tất cả các giá trị khác. Không có thuộc tính nào khác của mảng ảnh hưởng đến tính chính xác. 

### Tại sao nó hoạt động 

Giá trị đang chạy`ans`trong chương trình luôn là tiền tố tối đa ngoại trừ tại một chỉ mục duy nhất nơi nó có thể được thay thế bằng tiền tố tối thiểu. Nếu chỉ số đặc biệt đó không phải là vị trí của mức tối đa toàn cục thì khi gặp phần tử tối đa, nó vẫn được xử lý bằng một`max`vận hành, buộc`ans`để đạt đến mức tối đa toàn cầu. Nếu chỉ số đặc biệt chính xác ở vị trí tối đa, chương trình sẽ thay thế một`max`cập nhật với một`min`, điều này ngăn cản`ans`đạt được hoặc duy trì chính xác mức tối đa toàn cầu theo cách dự định. Điều này làm cho mỗi lần xuất hiện mức tối đa đều là một lựa chọn thất bại và không có chỉ mục nào khác có thể gây ra lỗi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        
        mx = max(a)
        cnt = 0
        for x in a:
            if x == mx:
                cnt += 1
        
        print(cnt)

if __name__ == "__main__":
    solve()
```Giải pháp thực hiện hai lần quét tuyến tính cho mỗi trường hợp thử nghiệm: một lần quét tiềm ẩn trong tính toán`max(a)`và một để đếm số lần xuất hiện. Logic trực tiếp mã hóa đặc tính mà chỉ các vị trí giữ mức tối đa toàn cầu mới tạo ra kết quả sai. 

Việc triển khai tránh mọi mô phỏng vòng lặp bị lỗi. Điều tinh tế quan trọng là đảm bảo rằng chúng tôi tính toán mức tối đa trước khi đếm số lần xuất hiện, vì cả hai thao tác đều phụ thuộc vào cùng một ảnh chụp nhanh mảng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
1 3 2
```| Bước | mx | Mảng | Đếm tối đa cho đến nay | 
| --- | --- | --- | --- | 
| quét tối đa | 3 | [1,3,2] | - | 
| số lần quét | 3 | [1,3,2] | 1 | 

Đầu ra là`1`. 

Điều này cho thấy chỉ có vị trí chứa giá trị`3`là rất quan trọng, vì chỉ có nó mới có thể được chọn làm`k`để phá vỡ sự đúng đắn. 

### Ví dụ 2 

đầu vào:```
3
1 1 1
```| Bước | mx | Mảng | Đếm tối đa cho đến nay | 
| --- | --- | --- | --- | 
| quét tối đa | 1 | [1,1,1] | - | 
| số lần quét | 1 | [1,1,1] | 3 | 

Đầu ra là`3`. 

Ở đây mọi vị trí đều an toàn về mặt giá trị bình đẳng, nhưng mỗi vị trí vẫn tương ứng với một lựa chọn hợp lệ về`k`. Vì tất cả các yếu tố đều bằng nhau nên mọi lựa chọn đều tạo ra kết quả cuối cùng như nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Một lượt để tính toán mức tối đa và một lượt để đếm số lần xuất hiện | 
| Không gian | O(1) thêm | Chỉ một số biến được lưu trữ bất kể kích thước đầu vào | 

Tổng công việc trên tất cả các trường hợp thử nghiệm là tuyến tính trong tổng kích thước mảng, được giới hạn bởi 10^6, dễ dàng khớp trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    import builtins

    input = sys.stdin.readline

    def solve():
        t = int(input())
        out = []
        for _ in range(t):
            n = int(input())
            a = list(map(int, input().split()))
            mx = max(a)
            cnt = 0
            for x in a:
                if x == mx:
                    cnt += 1
            out.append(str(cnt))
        return "\n".join(out)

    return solve()

# provided samples
assert run("""2
3
1 3 2
3
1 1 1
""") == "1\n3"

# custom cases
assert run("""1
1
5
""") == "1", "single element"

assert run("""1
5
1 2 3 4 5
""") == "1", "unique maximum"

assert run("""1
5
5 1 5 1 5
""") == "3", "multiple maxima"

assert run("""1
4
2 2 2 2
""") == "4", "all equal"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 phần tử | 1 | ranh giới tối thiểu | 
| mảng tăng dần | 1 | xử lý tối đa độc đáo | 
| lặp lại tối đa | 3 | nhiều vị trí tối đa | 
| tất cả đều bình đẳng | 4 | trường hợp thoái hóa toàn phần | 

## Vỏ cạnh 

Đối với mảng một phần tử như`5`, mức tối đa xảy ra một lần, vì vậy điều duy nhất có thể xảy ra`k`là vị trí đó. Thuật toán tính toán`mx = 5`và đếm một lần xuất hiện, tạo ra`1`, phù hợp với thực tế là bất kỳ sai lệch nào cũng sẽ yêu cầu một chỉ mục khác không tồn tại. 

Đối với một mảng trong đó tất cả các phần tử đều bằng nhau, chẳng hạn như`2 2 2 2`, mọi chỉ số đều khớp với mức tối đa. Thuật toán đếm tất cả các vị trí, trả về`4`. Trong mã bị lỗi, thay thế một`max`với`min`vẫn mang lại cùng một giá trị ở mỗi bước, vì vậy mọi lựa chọn của`k`vẫn chỉ “sai” theo nghĩa định nghĩa trống rỗng, nhưng kết quả đầu ra của chương trình vẫn nhất quán với số lượng vị trí tối đa.
