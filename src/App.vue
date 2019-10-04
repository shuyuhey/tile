<template>
  <div id="app">
    <button @click="addPicture">Add Picture</button>

    <PictureContainer>
      <LiquidContainer v-for="picture, index in pictures" :index="index" :length="pictures.length">
        <RemovableContainer>
          <Picture>
            <img :src="picture" alt="">
          </Picture>
        </RemovableContainer>
      </LiquidContainer>
    </PictureContainer>
  </div>
</template>

<script lang="ts">
import { Component, Vue } from "vue-property-decorator";
import styled from 'vue-styled-components';
import faker from "faker";

const RemovableContainer = styled.div`
  position: relative;
  width: 100%;
  height: 100%;

  &::before {
    content: "☓";
    background: white;
    font-weight: bold;
    position: absolute;
    top: 4px;
    right: 4px;
  }
`;

const LiquidContainer = styled('div', { index: Number, length: Number })`
  float: left;

  ${props => props.length == 1 && 'width: 100%;'}
  ${props => props.length > 1 && 'width: calc(50% - 2px);'}

  height: calc(var(--max-width) / 2 - 16px);
  ${props => props.index % 2 == 0 && 'margin-right: 4px;'}
  ${props => props.length !== 1 && props.length % 2 == 1 && props.index > (props.length - 3) && 'height: calc(var(--max-width) / 4 - 10px); margin-right: 0;'}

  margin-bottom: 4px;
  &:last-child {
    margin-bottom: 0;
  }
`;

const Picture = styled.div`
  width: 100%;
  height: 100%;
  overflow: hidden;
  background: gray;

  img {
    width: 100%;
    height: 100%;
    object-fit: cover;
  }
`;

const Container = styled.div`
  &::after {
    clear: both;
  }
`;

@Component({
  components: {
    PictureContainer: Container,
    Picture,
    RemovableContainer,
    LiquidContainer
  }
})
export default class App extends Vue {
  private pictures: string[] = [];


  addPicture(): void {
    const image = faker.image.avatar();

    this.pictures.push(image);
  }
}
</script>

<style>
  :root {
    --max-width: 375px;
  }
#app {
  max-width: 375px;
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin: 60px auto 0;
  padding: 16px;
  height: 100vh;
}
</style>
