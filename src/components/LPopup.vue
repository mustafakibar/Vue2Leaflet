<script>
import propsBinder from '../utils/propsBinder.js';
import findRealParent from '../utils/findRealParent.js';
import Popper from '../mixins/Popper.js';

export default {
  name: 'LPopup',
  mixins: [Popper],
  props: {
    latLng: {
      type: [Object, Array],
      default: () => []
    }
  },
  mounted () {
    this.mapObject = L.popup(this.popperOptions);
    if (this.latLng !== undefined) {
      this.mapObject.setLatLng(this.latLng);
    }
    L.DomEvent.on(this.mapObject, this.$listeners);
    propsBinder(this, this.mapObject, this.$options.props);
    this.mapObject.setContent(this.content || this.$el);
    this.parentContainer = findRealParent(this.$parent);
    this.parentContainer.mapObject.bindPopup(this.mapObject);
  },
  beforeDestroy () {
    if (this.parentContainer.mapObject && this.parentContainer.mapObject.getPopup()) {
      this.parentContainer.mapObject.unbindPopup();
    }
  }
};
</script>
